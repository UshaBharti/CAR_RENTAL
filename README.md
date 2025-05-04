# CAR_RENTAL
@Override
    public void addCar(Vehicle_Details car) throws SQLException, ClassNotFoundException {
    	

        connection = ConnectionHelper.getConnection();
        String cmd = "INSERT INTO vehicle (vehicleId, make, model, year, dailyRate, status, passengerCapacity, engineCapacity) "
                   + "VALUES (?, ?, ?, ?, ?, ?, ?, ?)";
        pstm = connection.prepareStatement(cmd);
        pstm.setInt(1, car.getVehicleId());
        pstm.setString(2, car.getMake());
        pstm.setString(3, car.getModel());
        pstm.setInt(4, car.getYear());
        pstm.setDouble(5, car.getDailyRate());
        pstm.setString(6, car.getStatus().toString());
        pstm.setInt(7, car.getPassengerCapacity());
        pstm.setDouble(8, car.getEngineCapacity());
        pstm.executeUpdate();
    }

    @Override
    public List<Vehicle_Details> showCar() throws SQLException, ClassNotFoundException {
        connection = ConnectionHelper.getConnection();
        String cmd = "SELECT * FROM vehicle";
        pstm = connection.prepareStatement(cmd);
        ResultSet rs = pstm.executeQuery();
        List<Vehicle_Details> details = new ArrayList<>();
        while (rs.next()) {
            Vehicle_Details car = new Vehicle_Details();
            car.setVehicleId(rs.getInt("vehicleID"));
            car.setMake(rs.getString("make"));
            car.setModel(rs.getString("model"));
            car.setYear(rs.getInt("year"));
            car.setDailyRate(rs.getDouble("dailyRate"));
            car.setStatus(Status.valueOf(rs.getString("status").toUpperCase()));
            car.setPassengerCapacity(rs.getInt("passengerCapacity"));
            car.setEngineCapacity(rs.getDouble("engineCapacity"));
            details.add(car);
        }
        return details;
    }

    @Override
    public void removeCar(int vehicleId) throws SQLException, ClassNotFoundException, CarNotFoundException {
        connection = ConnectionHelper.getConnection();
        String cmd = "DELETE FROM vehicle WHERE vehicleId = ?";
        pstm = connection.prepareStatement(cmd);
        pstm.setInt(1, vehicleId);
        int rows = pstm.executeUpdate();
        if (rows == 0) {
            throw new CarNotFoundException("Car with ID " + vehicleId + " not found.");
        }
    }

    @Override
    public List<Vehicle_Details> availabelCar() throws SQLException, ClassNotFoundException {
        connection = ConnectionHelper.getConnection();
        String cmd = "SELECT * FROM vehicle WHERE status = ?";
        pstm = connection.prepareStatement(cmd);
        pstm.setString(1, "available");
        ResultSet rs = pstm.executeQuery();
        List<Vehicle_Details> details = new ArrayList<>();
        while (rs.next()) {
            Vehicle_Details car = new Vehicle_Details();
            car.setVehicleId(rs.getInt("vehicleID"));
            car.setMake(rs.getString("make"));
            car.setModel(rs.getString("model"));
            car.setYear(rs.getInt("year"));
            car.setDailyRate(rs.getDouble("dailyRate"));
            car.setStatus(Status.valueOf(rs.getString("status").toUpperCase()));
            car.setPassengerCapacity(rs.getInt("passengerCapacity"));
            car.setEngineCapacity(rs.getDouble("engineCapacity"));
            details.add(car);
        }
        return details;
    }

    @Override
    public List<Vehicle_Details> rentedcar() throws SQLException, ClassNotFoundException {
        connection = ConnectionHelper.getConnection();
        String cmd = "SELECT * FROM vehicle WHERE status = ?";
        pstm = connection.prepareStatement(cmd);
        pstm.setString(1, "notAvailable");
        ResultSet rs = pstm.executeQuery();
        List<Vehicle_Details> details = new ArrayList<>();
        while (rs.next()) {
            Vehicle_Details car = new Vehicle_Details();
            car.setVehicleId(rs.getInt("vehicleID"));
            car.setMake(rs.getString("make"));
            car.setModel(rs.getString("model"));
            car.setYear(rs.getInt("year"));
            car.setDailyRate(rs.getDouble("dailyRate"));
            car.setStatus(Status.valueOf(rs.getString("status").toUpperCase()));
            car.setPassengerCapacity(rs.getInt("passengerCapacity"));
            car.setEngineCapacity(rs.getDouble("engineCapacity"));
            details.add(car);
        }
        return details;
    }

    @Override
    public Vehicle_Details findbyCarId(int vehicleId) throws SQLException, ClassNotFoundException, CarNotFoundException {
        connection = ConnectionHelper.getConnection();
        String cmd = "SELECT * FROM vehicle WHERE vehicleId = ?";
        pstm = connection.prepareStatement(cmd);
        pstm.setInt(1, vehicleId);
        ResultSet rs = pstm.executeQuery();
        if (!rs.next()) {
            throw new CarNotFoundException("Car with ID " + vehicleId + " not found.");
        }

        Vehicle_Details det = new Vehicle_Details();
        det.setVehicleId(rs.getInt("vehicleID"));
        det.setMake(rs.getString("make"));
        det.setModel(rs.getString("model"));
        det.setYear(rs.getInt("year"));
        det.setDailyRate(rs.getDouble("dailyRate"));
        det.setStatus(Status.valueOf(rs.getString("status").toUpperCase()));
        det.setPassengerCapacity(rs.getInt("passengerCapacity"));
        det.setEngineCapacity(rs.getDouble("engineCapacity"));
        return det;
    }
    /////
