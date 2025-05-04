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
    /////*******************************************
package com.java.car_rental.dao;

import java.sql.SQLException;
import java.util.List;

import com.java.car_rental.exception.CarNotFoundException;
import com.java.car_rental.exception.LeaseNotFoundException;
import com.java.car_rental.model.Lease_Details;
import com.java.car_rental.model.Payment_Details;


public interface Payment_Dao {
	public void recordPayment(Lease_Details lease) throws ClassNotFoundException, SQLException , LeaseNotFoundException, CarNotFoundException;
	List<Payment_Details> showpaymentRecord() throws SQLException, ClassNotFoundException;
}


***********************************************************************
package com.java.car_rental.dao;
import java.sql.Date;
import java.sql.SQLException;
import java.util.List;

import com.java.car_rental.exception.CarAlreadyBookedException;
import com.java.car_rental.exception.CustomerNotFoundException;
import com.java.car_rental.exception.LeaseNotFoundException;
import com.java.car_rental.model.Lease_Details;

public interface Lease_Dao {
	public void createLease(Lease_Details lease) throws SQLException, ClassNotFoundException,CustomerNotFoundException ;
	public Lease_Details searchleasebyId(int leaseId) throws SQLException, ClassNotFoundException, LeaseNotFoundException;
	List<Lease_Details> listActiveLease() throws SQLException, ClassNotFoundException;
	List<Lease_Details> showAllLease() throws SQLException, ClassNotFoundException;
	
}


****************************************************************************
package com.java.car_rental.dao;
import java.sql.SQLException;
import java.util.List;

import com.java.car_rental.exception.CustomerNotFoundException;
import com.java.car_rental.model.Customer_Details;
public interface Customer_Dao {
	public void addCustomer(Customer_Details customer) throws SQLException, ClassNotFoundException;
	public void removeCustomer(int customerId) throws SQLException, ClassNotFoundException, CustomerNotFoundException;
	List<Customer_Details> showCustomer()throws SQLException, ClassNotFoundException;
	public Customer_Details findCustomerId(int customerId)throws SQLException, ClassNotFoundException, CustomerNotFoundException;
}




***********************************************************************************
package com.java.car_rental.dao;
import java.sql.SQLException;
import java.time.LocalDate;
import java.util.List;

import com.java.car_rental.exception.CarAlreadyBookedException;
import com.java.car_rental.exception.CarNotFoundException;
import com.java.car_rental.model.Vehicle_Details;


public interface Car_Dao {
	public void addCar(Vehicle_Details car) throws SQLException, ClassNotFoundException;
	List<Vehicle_Details> showCar() throws SQLException, ClassNotFoundException;
	public void removeCar(int vehicleId) throws SQLException,ClassNotFoundException, CarNotFoundException;
	List<Vehicle_Details> availabelCar() throws SQLException, ClassNotFoundException;
	List<Vehicle_Details> rentedcar() throws SQLException, ClassNotFoundException;
	public Vehicle_Details findbyCarId(int vehicleId) throws SQLException, ClassNotFoundException, CarNotFoundException;
	public boolean isVehicleAvailable(int vehicleId,LocalDate startDate, LocalDate endDate) throws SQLException, ClassNotFoundException, CarNotFoundException,CarAlreadyBookedException;
}


*******************************************************
package com.java.car_rental.main;
import java.sql.Date;
import java.sql.SQLException;
import java.time.LocalDate;
import java.time.temporal.ChronoUnit;


import java.util.List;
import java.util.Scanner;

import com.java.car_rental.dao.Car_Dao;
import com.java.car_rental.dao.CardaoImpl;
import com.java.car_rental.dao.CustomerdaoImpl;
import com.java.car_rental.dao.Customer_Dao;
import com.java.car_rental.dao.LeasedaoImpl;
import com.java.car_rental.dao.Payment_Dao;
import com.java.car_rental.dao.PaymentdaoImpl;
import com.java.car_rental.exception.CarAlreadyBookedException;
import com.java.car_rental.exception.CarNotFoundException;
import com.java.car_rental.exception.CustomerNotFoundException;
import com.java.car_rental.exception.LeaseNotFoundException;
import com.java.car_rental.dao.Lease_Dao;
import com.java.car_rental.model.Customer_Details;
import com.java.car_rental.model.Lease_Details;
import com.java.car_rental.model.Payment_Details;
import com.java.car_rental.model.Status;
import com.java.car_rental.model.Vehicle_Details;

public class Main {
	static Scanner sc;
	static Car_Dao carDao;
	static Customer_Dao custDao;
	static Lease_Dao leaseDao;
	static Payment_Dao paymentDao;
	
	static {
		sc = new Scanner(System.in);
		carDao= new CardaoImpl();
		custDao= new CustomerdaoImpl();
		leaseDao = new LeasedaoImpl();
		paymentDao = new PaymentdaoImpl();
		}


	public static void main(String[] args) {
		
		int choice;
		do {
			System.out.println("\n Welcome To Car Rental System : \n");
			System.out.println("Enter The Choice:");
			System.out.println("Enter 1: Enter For adding Vehicle Details");
			System.out.println("Enter 2: Enter For Show Vehicle Details");
			System.out.println("Enter 3: Enter For Remove Vehicle");
			System.out.println("Enter 4: Enter For Available Vehicle");
			System.out.println("Enter 5: Enter For Rented Vehicle");
			System.out.println("Enter 6: Enter For Search the Vehicle");
			System.out.println("Enter 7: Enter For Adding Customer");
			System.out.println("Enter 8: Enter For Remove Customer");
			System.out.println("Enter 9: Enter for Show Customer");
			System.out.println("Enter 10: Enter For Search Customer");
			System.out.println("Enter 11: Enter For Creating A Lease");
			System.out.println("Enter 12: Enter For Search Leave by ID");
			System.out.println("Enter 13: Enter For See Active Lease");
			System.out.println("Enter 14: Enter For Show Lease");
			System.out.println("Enter 15: Enter For Payment Record");
			System.out.println("Enter 16: Enter For See Payment Records");
			System.out.println("Enter 0: Exit");
			 choice = sc.nextInt();
			
			try {
				switch(choice) {
					case 1: 
						addCarMain();
						break;
					case 2:
						showCarMain();
						break;
					case 3:
						removeCarMain();
						break;
					case 4:
						availableCarMain();
						break;
					case 5:
						rentedCarMain();
						break;
					case 6:
						searchVehicleMain();
						break;
					case 7:
						addCustomerMain();
						break;
					case 8:
						removeCustomerMain();
						break;
					case 9:
						showCustomerMain();
						break;
					case 10:
						searchCustomerMain();
						break;
					case 11:
						createLeaseMain();
						break;
					case 12:
						searchLeaseMain();
						break;
					case 13:
						activeLeaseMain();
						break;
					case 14:
						showLeaseMain();
						break;
					case 15:
						recordPaymentMain();
						break;
					case 16:
						showPaymentRecord();
						break;
					case 0:
                        System.out.println("Exiting...");
                        System.out.println("Thank You For Using Car Rental System......");
                        break;
                    default:
                        System.out.println("Invalid choice. Try again.");
                }
			} catch (ClassNotFoundException | SQLException e) {
                e.printStackTrace();
            } catch (CarNotFoundException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			} catch (CustomerNotFoundException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			} catch (CarAlreadyBookedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			} catch (LeaseNotFoundException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			
		}while(choice != 0);
	}

	public static void showPaymentRecord()throws SQLException, ClassNotFoundException {
		List<Payment_Details> details = paymentDao.showpaymentRecord();
		for (Payment_Details dt : details) {
			System.out.println(dt);
		}
		
	}

	public static void recordPaymentMain() throws SQLException, ClassNotFoundException, LeaseNotFoundException, CarNotFoundException{
		System.out.println("Enter Lease Id:");
		int id = sc.nextInt();
		    try {
		        Lease_Details details = leaseDao.searchleasebyId(id);
		        paymentDao.recordPayment(details);
		        System.out.println("✅ Payment recorded successfully.");

		    } catch (LeaseNotFoundException e) {
		        System.err.println("❌ Error: " +e.getMessage());
		    } catch (CarNotFoundException e) {
		        System.err.println("❌ " + e.getMessage());
		    } catch (ClassNotFoundException | SQLException e) {
		        System.err.println(" ❌ An unexpected error occurred.");
		        e.printStackTrace();
		    }
		}


	public static void activeLeaseMain() throws SQLException, ClassNotFoundException {
		List<Lease_Details> details = leaseDao.listActiveLease();
		for (Lease_Details dt : details) {
			System.out.println(dt);
		}	
	}

	public static void showLeaseMain() throws SQLException, ClassNotFoundException{
		List<Lease_Details> details = leaseDao.showAllLease();
		for (Lease_Details dt : details) {
			System.out.println(dt);
		}
	}

	public static void searchLeaseMain() throws SQLException, ClassNotFoundException, LeaseNotFoundException{
		System.out.println("Enter LeaseId:");
		int id = sc.nextInt();
		
		try {
			Lease_Details details= leaseDao.searchleasebyId(id);
			System.out.println("✅ Lease Details Found");
			System.out.println(details);
		    }catch (LeaseNotFoundException e) {
		        System.err.println("❌ Error: " + e.getMessage());
		    }catch (ClassNotFoundException | SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
	}

	public static void createLeaseMain() throws SQLException, ClassNotFoundException, CarAlreadyBookedException, CustomerNotFoundException {
		
		 try {
		        System.out.println("Enter Lease Id");
		        int leaseId = sc.nextInt();

		        System.out.print("Enter Customer ID: ");
		        int customerId = sc.nextInt();

		        System.out.print("Enter Vehicle ID: ");
		        int vehicleId = sc.nextInt();

		        sc.nextLine(); // Consume the leftover newline

		        System.out.print("Enter Start Date (yyyy-mm-dd): ");
		        String startInput = sc.nextLine();

		        System.out.print("Enter End Date (yyyy-mm-dd): ");
		        String endInput = sc.nextLine();
		        
		        LocalDate startLocal = LocalDate.parse(startInput);
		        LocalDate endLocal = LocalDate.parse(endInput);

		        Date startDate = Date.valueOf(startLocal);
		        Date endDate = Date.valueOf(endLocal);

		        long diffDays = ChronoUnit.DAYS.between(startLocal, endLocal);
		        String type;
		        if (diffDays >= 15) {
		            type = "MonthlyLease";
		        } else {
		            type = "DailyLease";
		        }
		        
		        System.out.println("Lease type being set: " + type);

		        // Check if the vehicle is available first (before calling createLease)
		        boolean isAvailable = new CardaoImpl().isVehicleAvailable(vehicleId, startLocal, endLocal);
		        
		        if (!isAvailable) {
		            throw new CarAlreadyBookedException("❌ Vehicle with ID " + vehicleId + " is already booked for the selected period.");
		        }

		        // If vehicle is available, create the lease
		        Lease_Details lease = new Lease_Details(leaseId,vehicleId,customerId, startDate, endDate, type);
		        leaseDao.createLease(lease);

		        if (lease != null) {
		            System.out.println("\n✅ Lease Created Successfully:");
		            System.out.println(lease);
		        } else {
		            System.out.println("\n❌ Lease creation failed.");
		        }

		    } catch (CarAlreadyBookedException e) {
		        // Handle case where the car is already booked
		        System.out.println("❌ Error: " + e.getMessage());
		    }catch (CustomerNotFoundException e) {
		        System.err.println(e.getMessage());
		    } catch (SQLException | ClassNotFoundException e) {
		        // Handle SQL or database connection errors
		        System.out.println("❌ Database error: " + e.getMessage());
		    } catch (IllegalArgumentException e) {
		        // Handle invalid arguments (such as invalid dates)
		        System.out.println("❌ Error: " + e.getMessage());
		    } catch (Exception e) {
		        // Catch any other unexpected exceptions
		        System.out.println("❌ Unexpected error: " + e.getMessage());
		    }
	}

	public static void searchCustomerMain() throws SQLException, ClassNotFoundException{
		System.out.println("Enter The Customer Id");
		int id = sc.nextInt();
		
		try {
			Customer_Details details = custDao.findCustomerId(id);
			System.out.println("Customer Details Found");
			System.out.println(details);
		} catch (CustomerNotFoundException e) {
	        // Handle the case where the customer was not found
	        System.err.println("❌ Error: " + e.getMessage());
		}catch (ClassNotFoundException | SQLException  e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
	}

	public static void showCustomerMain() throws SQLException, ClassNotFoundException {
		List<Customer_Details> details= custDao.showCustomer();
		
		for(Customer_Details dt : details) {
			System.out.println(dt);
		}
		
		
	}

	public  static void removeCustomerMain() throws SQLException, ClassNotFoundException, CustomerNotFoundException  {
		System.out.println("Enter The Customer Id:");
		int id = sc.nextInt();
		try {
			custDao.removeCustomer(id);
			System.out.println("✅ Customer"+ " " +id+ " " +"Removed Successfully");
		}catch (CustomerNotFoundException e) {
	        // Handle the case where the customer was not found
	        System.err.println("❌ Error: " + e.getMessage());
		}catch (ClassNotFoundException | SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}

	public static void addCustomerMain() throws SQLException, ClassNotFoundException {
		
		System.out.println("Enter Customer Id");
		int id = sc.nextInt();
		
		System.out.println("Enter First Name:");
		String FirstName =  sc.next();
		
		System.out.println("Enter Last Name:");
		String LastName= sc.next();
		
		System.out.println("Enter Email ID:");
		String Email = sc.next();
		
		System.out.println("Enter The Mobile No:");
		String MobileNo= sc.next();
		
		Customer_Details details = new Customer_Details(id,FirstName, LastName,Email,MobileNo);
		custDao.addCustomer(details);
		 System.out.println("✅ Customer added successfully!");
		
		
	}

	public static void searchVehicleMain() throws SQLException, ClassNotFoundException, CarNotFoundException {
		System.out.println("Enter The Vehicle Id");
		int id = sc.nextInt();
		try {
			Vehicle_Details vehicle = carDao.findbyCarId(id);
			System.out.println("🚗 Vehicle Details Found:");
	        System.out.println(vehicle);
		} catch (CarNotFoundException e) {
	        System.err.println("❌ Error: " + e.getMessage());
	    }catch (ClassNotFoundException | SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
	}

	public static void rentedCarMain() throws SQLException, ClassNotFoundException {
	    List<Vehicle_Details> rentedCars = carDao.rentedcar();
	    if (rentedCars.isEmpty()) {
	        System.out.println("❌ No rented cars at the moment.");
	    } else {
	        System.out.println("✅ Rented Cars:");
	        for (Vehicle_Details car : rentedCars) {
	            System.out.println(car);
	        }
	    }
	}


	public static void availableCarMain()throws SQLException, ClassNotFoundException {
		 List<Vehicle_Details> availableCars = carDao.availabelCar();
		    if (availableCars.isEmpty()) {
		        System.out.println("❌ No available cars at the moment.");
		    } else {
		        System.out.println("✅ Available Cars:");
		        for (Vehicle_Details car : availableCars) {
		            System.out.println(car);
		        }
		    }
	}

	public static void removeCarMain() throws SQLException, ClassNotFoundException,CarNotFoundException{
		System.out.println("Enter The Vehicle Id:");
		int vehicleid = sc.nextInt();
		
		try {
			carDao.removeCar(vehicleid);
			System.out.println("✅ Vehicle"+ " " +vehicleid+ " " +"Removed Successfully");
		}
			catch (CarNotFoundException e) {
		        System.err.println("❌ Error: " + e.getMessage());
		} catch (ClassNotFoundException | SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}

	public static void showCarMain() throws ClassNotFoundException, SQLException {
		List<Vehicle_Details> showcar = carDao.showCar();
		for(Vehicle_Details dt : showcar) {
			System.out.println(dt);
		}
	}

	public static void addCarMain() throws ClassNotFoundException, SQLException{	
		
		System.out.println("Enter Vehicle Id");
		int id = sc.nextInt();
		sc.nextLine();
		
        System.out.print("Enter Make: ");
        String make = sc.nextLine();
        
        System.out.print("Enter Model: ");
        String model = sc.nextLine();

        System.out.print("Enter Year: ");
        int year = sc.nextInt();

        System.out.print("Enter Daily Rate: ");
        double dailyRate = sc.nextDouble();
        sc.nextLine();

        System.out.print("Enter Status (AVAILABLE/NOTAVAILABLE): ");
        String statusInput = sc.nextLine();
        Status status = Status.valueOf(statusInput); // will throw exception if invalid

        System.out.print("Enter Passenger Capacity: ");
        int passengerCapacity = sc.nextInt();

        System.out.print("Enter Engine Capacity (in cc): ");
        double engineCapacity = sc.nextDouble();

        Vehicle_Details car = new Vehicle_Details(id,make, model, year, dailyRate,status, passengerCapacity, engineCapacity);
        carDao.addCar(car);
        System.out.println("✅ Car added successfully!");
		
	}
}
********************************************************8
package com.java.car_rental.dao;
import java.sql.Connection;
import java.sql.Date;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.time.LocalDate;
import java.time.temporal.ChronoUnit;
import java.util.ArrayList;
import java.util.List;

import com.java.car_rental.exception.CarNotFoundException;
import com.java.car_rental.exception.LeaseNotFoundException;
import com.java.car_rental.model.Lease_Details;
import com.java.car_rental.model.Payment_Details;
import com.java.car_rental.util.ConnectionHelper;

public class PaymentdaoImpl implements Payment_Dao {
	
	Connection conn;
	PreparedStatement pstm;
	
	@Override
	public void recordPayment(Lease_Details lease) throws ClassNotFoundException, SQLException , LeaseNotFoundException, CarNotFoundException{
	    conn = ConnectionHelper.getConnection();

	    String leaseCheck = "SELECT leaseId FROM lease WHERE leaseId = ?";
	    pstm = conn.prepareStatement(leaseCheck);
	    pstm.setInt(1, lease.getLeaseId());
	    ResultSet leaseRs = pstm.executeQuery();
	    if (!leaseRs.next()) {
	        throw new LeaseNotFoundException("Lease with ID " + lease.getLeaseId() + " not found.");
	    }

	    LocalDate startDate = lease.getStartDate().toLocalDate();
	    LocalDate endDate = lease.getEndDate().toLocalDate();
	    long days = ChronoUnit.DAYS.between(startDate, endDate);
	    if (days == 0) days = 1;

	    
	    String rate = "SELECT dailyRate FROM vehicle WHERE vehicleId = ?";
	    pstm = conn.prepareStatement(rate);
	    pstm.setInt(1, lease.getVehicleId());
	    ResultSet rateRs = pstm.executeQuery();

	   
	    // Record payment
	    String paymentSql = "INSERT INTO payment (leaseId, amount, paymentDate) VALUES (?, ?, ?)";
	    pstm = conn.prepareStatement(paymentSql);
	    pstm.setInt(1, lease.getLeaseId());
	    pstm.setDouble(2, amount);
	    pstm.setDate(3, Date.valueOf(LocalDate.now()));
	    pstm.executeUpdate();
	}

	@Override
	public List<Payment_Details> showpaymentRecord() throws SQLException, ClassNotFoundException {
		List<Payment_Details> list = new ArrayList<Payment_Details>();
		conn= ConnectionHelper.getConnection();
		String cmd = "Select * from payment";
		pstm= conn.prepareStatement(cmd);
		ResultSet rs = pstm.executeQuery();
		while(rs.next()) {
			Payment_Details details = new Payment_Details();
			details.setLeaseId(rs.getInt("leaseId"));
			details.setTotalAmt(rs.getDouble("amount"));
			details.setPaymentDate(rs.getDate("paymentDate"));
			list.add(details);
		}
		return list;
	}
	

}


********************************************
package com.java.car_rental.dao;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.time.LocalDate;

import java.util.ArrayList;
import java.util.List;

import com.java.car_rental.exception.CustomerNotFoundException;
import com.java.car_rental.exception.LeaseNotFoundException;
import com.java.car_rental.model.Lease_Details;
import com.java.car_rental.util.ConnectionHelper;



public class LeasedaoImpl implements Lease_Dao{
	
	Connection conn;
	PreparedStatement pstm;

	@Override
	public void createLease(Lease_Details lease) throws SQLException, ClassNotFoundException, CustomerNotFoundException{

		 
		conn = ConnectionHelper.getConnection();
		
		 
		 String cmd = "INSERT INTO lease (leaseId, vehicleID, customerID, startDate, endDate, type) VALUES (?, ?, ?, ?, ?, ?)";
	       pstm = conn.prepareStatement(cmd);
	       pstm.setInt(1, lease.getLeaseId()); 
	       pstm.setInt(2, lease.getVehicleId());
	       pstm.setInt(3, lease.getCustomerId());
	       pstm.setDate(4, lease.getStartDate());
	       pstm.setDate(5, lease.getEndDate());
	       pstm.setString(6, lease.getType());
	       pstm.executeUpdate();
	}

	@Override
	public Lease_Details searchleasebyId(int leaseId) throws SQLException, ClassNotFoundException, LeaseNotFoundException {
		conn = ConnectionHelper.getConnection();
		String cmd = "Select * from lease where leaseId = ?";
		pstm = conn.prepareStatement(cmd);
		pstm.setInt(1, leaseId);
		ResultSet rs = pstm.executeQuery();
		Lease_Details details = new Lease_Details();
		 if (rs.next()) {
		        details.setLeaseId(rs.getInt("leaseId"));
		        details.setVehicleId(rs.getInt("vehicleID"));
		        details.setCustomerId(rs.getInt("customerID"));
		        details.setStartDate(rs.getDate("startDate"));
		        details.setEndDate(rs.getDate("endDate"));
		        details.setType(rs.getString("type"));
		        return details;
		    } else {
		        throw new LeaseNotFoundException(" No lease found with Lease ID: " + leaseId);
		    }
	}

	@Override
	public List<Lease_Details> showAllLease() throws SQLException, ClassNotFoundException {
		conn = ConnectionHelper.getConnection();
		String cmd = "Select * from lease";
		pstm =conn.prepareStatement(cmd);
		List<Lease_Details> list = new ArrayList<Lease_Details>();
		ResultSet rs = pstm.executeQuery();
		while(rs.next()) {
			Lease_Details details = new Lease_Details();
			details.setLeaseId(rs.getInt("leaseId"));
	        details.setVehicleId(rs.getInt("vehicleID"));
	        details.setCustomerId(rs.getInt("customerID"));
	        details.setStartDate(rs.getDate("startDate"));
	        details.setEndDate(rs.getDate("endDate"));
	        details.setType(rs.getString("type"));
	        list.add(details);
		}
		return list;
	}

	@Override
	public List<Lease_Details> listActiveLease() throws SQLException, ClassNotFoundException {	
		    conn = ConnectionHelper.getConnection();
		    String cmd = "SELECT * FROM lease WHERE startDate <= CURDATE() AND endDate >= CURDATE()";
		    List<Lease_Details> activeLeases = new ArrayList<>();
		    pstm = conn.prepareStatement(cmd);
		    ResultSet rs = pstm.executeQuery();

		    while (rs.next()) {
		        Lease_Details lease = new Lease_Details();
		        lease.setLeaseId(rs.getInt("leaseID"));
		        lease.setVehicleId(rs.getInt("vehicleID"));
		        lease.setCustomerId(rs.getInt("customerID"));
		        lease.setStartDate(rs.getDate("startDate"));
		        lease.setEndDate(rs.getDate("endDate"));
		        lease.setType(rs.getString("type"));

		        activeLeases.add(lease);
		    }

		    return activeLeases;
		}
 

}

*****************************************************
package com.java.car_rental.dao;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

import com.java.car_rental.exception.CustomerNotFoundException;
import com.java.car_rental.model.Customer_Details;
import com.java.car_rental.util.ConnectionHelper;


public class CustomerdaoImpl implements Customer_Dao {
	
	Connection conn;
	PreparedStatement pstm;
	@Override
	public void addCustomer(Customer_Details customer) throws SQLException, ClassNotFoundException {
		conn= ConnectionHelper.getConnection();
		String cmd = "Insert into customer(customerId,firstName,lastName,email,phoneNumber) values (?,?,?,?,?)";
		pstm = conn.prepareStatement(cmd);
		pstm.setInt(1, customer.getCustomerId());
		pstm.setString(2, customer.getFName());
		pstm.setString(3, customer.getLName());
		pstm.setString(4, customer.getEmail());
		pstm.setString(5, customer.getPhNo());
		
		pstm.executeUpdate();
		
	}
	@Override
	public void removeCustomer(int customerId) throws SQLException,ClassNotFoundException, CustomerNotFoundException {
		conn = ConnectionHelper.getConnection();
		String cmd = "Delete from customer where customerID = ?";
		pstm= conn.prepareStatement(cmd);
		pstm.setInt(1, customerId);
		int rows = pstm.executeUpdate();
        if (rows == 0) {
            throw new CustomerNotFoundException("Customer with ID " + customerId + " not found.");
        }
		
	}
	@Override
	public List<Customer_Details> showCustomer() throws SQLException, ClassNotFoundException {
		conn = ConnectionHelper.getConnection();
		String cmd = "Select * from customer";
		pstm = conn.prepareStatement(cmd);
		List<Customer_Details> details =  new ArrayList<Customer_Details>();
		ResultSet rs = pstm.executeQuery();
		while(rs.next()) {
			Customer_Details list = new Customer_Details();
			list.setCustomerId(rs.getInt("customerID"));
			list.setFName(rs.getString("firstName"));
			list.setLName(rs.getString("lastName"));
			list.setEmail(rs.getString("email"));
			list.setPhNo(rs.getString("phoneNumber"));
			details.add(list);
		}
		return details;
	}
	@Override
	public Customer_Details findCustomerId(int customerId) throws SQLException, ClassNotFoundException, CustomerNotFoundException {
		Customer_Details details = null;
		conn = ConnectionHelper.getConnection();
		String cmd =  "SELECT * FROM customer WHERE customerId = ?";
		pstm = conn.prepareStatement(cmd);
		pstm.setInt(1, customerId);
		ResultSet rs = pstm.executeQuery();
		
		if (!rs.next()) {
	        throw new CustomerNotFoundException("Customer with ID " + customerId + " not found.");
	    }
			details = new Customer_Details();
			details.setCustomerId(rs.getInt("customerID"));
			details.setFName(rs.getString("firstName"));
			details.setLName(rs.getString("lastName"));
			details.setEmail(rs.getString("email"));
			details.setPhNo(rs.getString("phoneNumber"));  // Correct type
		
		return details;
	}

}
***************************************************@Override
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

*********************************************************
