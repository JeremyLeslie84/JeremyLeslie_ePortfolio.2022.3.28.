# ***Introduction***

Hello,

My name is Jeremy Leslie and this ePortfolio has been created to display some of the skills that I have developed throughout the completion of the BS in computer science with a concentration in software engineering program at Southern New Hampshire University. It contains the source code for several applications that I have developed to meet the requirements of the program. Each application includes a brief summary, which is then followed by the application source code.



# ***Application Summary/Introduction - Contact/Appointment Management Application.***

The following JAVA source code is for a simple contact/appointment management application that allows users to perform CRUD functionality on both contacts and appointments. I have included it in this ePortfolio to demonstrate the following skills: object orientated programming techniques, input checking consideration, exception handling, file read/write algorithms, simple data structure implementation, string and date formatting, and evaluation statements.

## Source Code.

- ### Main Class.

```java
/*
 * Grand Strand System - Main class.
 * Developed by Jeremy Leslie 11/15/21.
 * Main Class Implements:
 * -Data management tasks for the Contacts and Appointments classes.
 */

package main;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
import AppointmentService.Appointment;
import AppointmentService.AppointmentService;
import ContactService.Contact;
import ContactService.ContactService;

public class Main {
	// Global variables for the main class.
	static String contactFirstName;
	static String contactLastName;

	// Buffer reader object to read user inputs.
	static BufferedReader bufferReader = new BufferedReader(new InputStreamReader(System.in));

	public static void main(String[] args) throws IOException, ParseException {
		// Local variables for main class.
		String userInput = "";
		String contactPhoneNumber;
		String contactStreetAddress;
		String contactCity;
		String contactState;
		String contactZIP;
		String appointmentDescription;
		String appointmentID;
		SimpleDateFormat simpleDateFormat = new SimpleDateFormat("MM/dd/yyyy");
		Date appointmentDate = new Date();

		// Create new contactService object to manage contacts.
		ContactService contactService = new ContactService();
		// Create new appointmentService object to manage appointments.
		AppointmentService appointmentService = new AppointmentService();
		// Load contact data from .csv file.
		contactService.readContactsFromDisk();
		// Load appointment data from .csv file.
		appointmentService.readAppointmentsFromDisk();
		// Print welcome message.
		System.out.println("WELCOME TO THE GRAND STRAND APPOINTMENT MANAGMENT SYSTEM");
		// Top level user input loop. Loop until user enters 1.
		while (!userInput.equals("1")) {
			System.out.print("INPUT OPTIONS:\n1 - TO EXIT\n2 - TO SERVICE CONTACTS\n3 - TO SERVICE APPOINTMENTS\n-> ");
			userInput = bufferReader.readLine().toUpperCase();
			switch (userInput) {
			// Exit case.
			case "1":
				System.out.println("Exiting...");
				break;
			// Service Contact case.
			case "2":
				userInput = "";
				// User input loop for service contact menu. Loop until user enters X.
				while (!userInput.equalsIgnoreCase("X")) {
					System.out.print(
							"SERVICE CONTACTS - INPUT OPTIONS:\nX - TO GO BACK\n1 - TO PRINT CONTACT LIST BY LAST NAME\n2 - TO CREATE NEW CONTACT"
									+ "\n3 - TO DELETE EXISTING CONTACT\n4 - TO UPDATE FIRST NAME\n5 - TO UPDATE LAST NAME"
									+ "\n6 - TO UPDATE STREET ADDRESS\n7 - TO UPDATE CITY\n8 - TO UPDATE STATE\n9 - TO UPDATE ZIP\n-> ");
					userInput = bufferReader.readLine().toUpperCase();
					switch (userInput) {
					// Exit case.
					case "X":
						break;
					// Print contact list case.
					case "1":
						System.out.println("CONTACT LIST:");
						System.out.println("LAST NAME:  FIRST NAME: ID:");
						contactService.printContactListByLastName();
						break;
					// Create new contact case. If contact already in list print error else add
					// contact to list.
					case "2":
						System.out.println("CREATE NEW CONTACT:");
						getContactName();
						String contactID = contactService.getContactID(contactLastName, contactFirstName);
						// If the contact is found in list print message.
						if (contactService.searchContactList(contactID)) {
							System.out.println("CONTACT ALREADY EXISTS");
						// If the contact is not found read inputs from buffer and create new contact.
						} else {
							System.out.println("ENTER PHONE # (MUST BE 10 CHARACTERS - NO DASHES) -> ");
							contactPhoneNumber = bufferReader.readLine().toUpperCase();
							System.out.println("ENTER STREET ADDRESS (UP TO 50 CHARACTERS) -> ");
							contactStreetAddress = bufferReader.readLine().toUpperCase();
							System.out.println("ENTER CITY (UP TO 30 CHARACTERS) -> ");
							contactCity = bufferReader.readLine().toUpperCase();
							System.out.println("ENTER STATE (MUST BE 2 CHARACTERS) -> ");
							contactState = bufferReader.readLine().toUpperCase();
							System.out.println("ENTER ZIP (MUST BE 5 CHARACTERS) -> ");
							contactZIP = bufferReader.readLine().toUpperCase();
							Contact contact_add = new Contact(contactService.getNextContactID(), contactFirstName,
									contactLastName, contactPhoneNumber, contactStreetAddress, contactCity,
									contactState, contactZIP);
							contactService.addContact(contact_add);
						}
						break;
					// Delete existing contact case.
					case "3":
						System.out.println("DELETE CONTACT:");
						getContactName();
						contactService.deleteContact(contactLastName, contactFirstName);
						break;
					// Update first name case.
					case "4":
						System.out.println("UPDATE CONTACT FIRST NAME:");
						getContactName();
						System.out.println("ENTER NEW FIRST NAME");
						String updatedContactFirstName = bufferReader.readLine().toUpperCase();
						;
						contactService.updateContactFirstName(
								contactService.getContactID(contactLastName, contactFirstName),
								updatedContactFirstName);
						break;
					// Update last name case.
					case "5":
						System.out.println("UPDATE CONTACT LAST NAME:");
						getContactName();
						System.out.println("ENTER NEW LAST NAME");
						String updatedContactLastName = bufferReader.readLine().toUpperCase();
						contactService.updateContactLastName(
								contactService.getContactID(contactLastName, contactFirstName), updatedContactLastName);
						break;
					// Update street address case.
					case "6":
						System.out.println("UPDATE CONTACT STREET ADDRESS:");
						getContactName();
						System.out.println("ENTER NEW STREET ADDRESSS");
						String updatedStreetAddress = bufferReader.readLine().toUpperCase();
						;
						contactService.updateContactStreetAddress(
								contactService.getContactID(contactLastName, contactFirstName), updatedStreetAddress);
						break;
					// Update city case.
					case "7":
						System.out.println("UPDATE CONTACT CITY:");
						getContactName();
						System.out.println("ENTER NEW CITY");
						String updatedCity = bufferReader.readLine().toUpperCase();
						;
						contactService.updateContactCity(contactService.getContactID(contactLastName, contactFirstName),
								updatedCity);
						break;
					// Update state case.
					case "8":
						System.out.println("UPDATE CONTACT STATE:");
						getContactName();
						System.out.println("ENTER NEW STATE");
						String updatedState = bufferReader.readLine().toUpperCase();
						;
						contactService.updateContactState(
								contactService.getContactID(contactLastName, contactFirstName), updatedState);
						break;
					// Update ZIP case.
					case "9":
						System.out.println("UPDATE CONTACT ZIP:");
						getContactName();
						System.out.println("ENTER NEW ZIP");
						String updatedZIP = bufferReader.readLine().toUpperCase();
						;
						contactService.updateContactZIP(contactService.getContactID(contactLastName, contactFirstName),
								updatedZIP);
						break;
					// Default case for non-defined inputs.
					default:
						System.out.println("INVALID INPUT");
						break;
					}
					// Sort and write (update) contact list to disk.
					contactService.sortContactListByID();
					contactService.writeContactsToDisk();
				}
				break;
			// Service Appointment case.
			case "3":
				userInput = "";
				// User input loop for service appointment menu. Loop until user enters X.
				while (!userInput.equalsIgnoreCase("X")) {
					System.out.print(
							"SERVICE APPOINTMENTS - INPUT OPTIONS:\nX - TO GO BACK\n1 - TO PRINT APPOINTMENT LIST\n2 - TO CREATE NEW APPOINTMENT"
									+ "\n3 - TO DELETE EXISTING APPOINTMENT\n4 - TO UPDATE APPOINTMENT DATE\n5 - TO UPDATE APPOINTMENT DESCRIPTION\n-> ");
					userInput = bufferReader.readLine().toUpperCase();

					switch (userInput) {
					// Exit case.
					case "X":
						break;
					// Print appointment list case.
					case "1":
						getContactName();
						System.out.println("APPOINTMENTS FOR: " + contactLastName + ", " + contactFirstName);
						System.out.println("ID           DATE:        DESCRIPTION:");
						appointmentService.printAppointmentList(contactFirstName, contactLastName);
						break;
					// Create new appointment case.
					case "2":
						System.out.println("ADD APPOINTMENT:");
						getContactName();
						// If the contact is found in list print message.
						String contactID = contactService.getContactID(contactLastName, contactFirstName);
						if (!contactService.searchContactList(contactID)) {
							System.out.println("CONTACT DOES NOT EXISTS");
						// If the appointment is not found read inputs from buffer and create new
						// appointment.
						} else {
							System.out.println("ENTER DATE(MM/DD/YYYY) -> ");
							appointmentDate = simpleDateFormat.parse(bufferReader.readLine().toUpperCase());
							System.out.println("APPOINTMENT DESCRIPTION (UP TO 50 CHARACTERS) -> ");
							appointmentDescription = bufferReader.readLine().toUpperCase();
							Appointment appointment_add = new Appointment(appointmentService.getNextAppointmentID(),
									appointmentDate, appointmentDescription, contactFirstName, contactLastName);
							appointmentService.addAppointment(appointment_add);
						}
						break;
					// Delete existing appointment case.
					case "3":
						System.out.println("DELETE APPOINTMENT:");
						System.out.println("ENTER APPOINTMENT ID -> ");
						appointmentService.deleteAppointment(bufferReader.readLine().toUpperCase());
						break;
					// Update appointment date case.
					case "4":
						System.out.println("UPDATE APPOINTMENT DATE:");
						System.out.println("ENTER APPOINTMENT ID -> ");
						appointmentID = bufferReader.readLine().toUpperCase();
						System.out.println("ENTER DATE(MM/DD/YYYY) -> ");
						appointmentDate = simpleDateFormat.parse(bufferReader.readLine().toUpperCase());
						appointmentService.updateAppointmentDate(appointmentID, appointmentDate);
						break;
					// Update appointment description case.
					case "5":
						System.out.println("UPDATE APPOINTMENT DESCRIPTION:");
						System.out.println("ENTER APPOINTMENT ID -> ");
						appointmentID = bufferReader.readLine().toUpperCase();
						System.out.println("ENTER NEW DESCRIPTION -> ");
						appointmentDescription = bufferReader.readLine().toUpperCase();
						appointmentService.updateAppointmentDescription(appointmentID, appointmentDescription);
						break;
					default:
						System.out.println("INVALID INPUT");
						break;
					}
					// Sort and write (update) appointment list to disk.
					appointmentService.sortAppointmentListByID();
					appointmentService.writeAppointmentsToDisk();
				}
				break;
			default:
				System.out.println("INVALID INPUT");
				break;
			}
		}
	}

	// Method reads user name from buffer.
	private static void getContactName() throws IOException {
		System.out.println("ENTER FIRST NAME (UP TO 20 CHARACTERS) -> ");
		contactFirstName = bufferReader.readLine().toUpperCase();
		System.out.println("ENTER LAST NAME (UP TO 20 CHARACTERS) -> ");
		contactLastName = bufferReader.readLine().toUpperCase();
	}
}
```

- ### Contact Class.

```java
/*
 * Grand Strand System - Contact class.
 * Developed by Jeremy Leslie 11/15/21.
 * Contact class implements:
 * -Contact variables
 * -Setter and getter methods for the Contacts class.
 */

package ContactService;

public class Contact {
	
	// Class variables.
	String contactID;
	String contactFirstName;
	String contactLastName;
	String contactPhoneNumber;
	String contactStreetAddress;
	String contactCity;
	String contactState;
	String contactZIP;
	
	// Contact object constructor.
	public Contact(String contactID, String contactFirstName, String contactLastName, String contactPhoneNumber, String contactStreetAddress,
			String contactCity, String contactState, String contactZIP) {
		
		// Check if contact Id meets input criteria, if not throw exception error.
		if (contactID == null || contactID.length() != 10 || !checkStringIsNumeric(contactID)) {		
			throw new IllegalArgumentException("Invalid ID");
		}
		
		// Check if first name meets input criteria, if not throw exception error.
		if (contactFirstName == null || contactFirstName.length() > 12) {
			throw new IllegalArgumentException("Invalid first name");
		}
		
		// Check if last name meets input criteria, if not throw exception error.
		if (contactLastName == null || contactLastName.length() > 12) {
			throw new IllegalArgumentException("Invalid last name");
		}
		
		// Check if phone # meets input criteria, if not throw exception error.
		if (contactPhoneNumber == null || contactPhoneNumber.length() != 10 || !checkStringIsNumeric(contactPhoneNumber)) {
			throw new IllegalArgumentException("Invalid phone number");
		}
		
		// Check if street address meets input criteria, if not throw exception error.
		if (contactStreetAddress == null || contactStreetAddress.length() > 30) {
			throw new IllegalArgumentException("Invalid street address");
		}
		
		// Check if city meets input criteria, if not throw exception error.
		if (contactCity == null || contactCity.length() > 15) {
			throw new IllegalArgumentException("Invalid city");
		}
		
		// Check if state meets input criteria, if not throw exception error.
		if (contactState == null || contactState.length() != 2) {
			throw new IllegalArgumentException("Invalid state");
		}
		
		// Check if ZIP meets input criteria, if not throw exception error.
		if (contactZIP == null || contactZIP.length() < 4 || contactZIP.length() > 5 || !checkStringIsNumeric(contactZIP)) {
			throw new IllegalArgumentException("Invalid ZIP");
		}

		// Set the current contact object variables.
		this.contactID = contactID;
		this.contactFirstName = contactFirstName;
		this.contactLastName = contactLastName;
		this.contactPhoneNumber = contactPhoneNumber;
		this.contactStreetAddress = contactStreetAddress;
		this.contactCity = contactCity;
		this.contactState = contactState;
		this.contactZIP = contactZIP;
	}
	 
	// Setter methods for the contact class.
	public void setContactFirstName(String contactFirstName) {
		// Check if contact first name meets input criteria, if not throw exception error.
		if(!contactFirstName.isEmpty() && contactFirstName.length() <= 20) {
			this.contactFirstName = contactFirstName;
		}
		else {
			throw new IllegalArgumentException("Invalid First Name");
		}
	}
	
	public void setContactLastName(String contactLastName) {
		// Check if contact last name meets input criteria, if not throw exception error.
		if(!contactLastName.isEmpty() && contactLastName.length() <= 20) {
			this.contactLastName = contactLastName;
		}
		else {
			throw new IllegalArgumentException("Invalid Last Name");
		}
	}
	
	public void setContactPhoneNumber(String contactPhoneNumber) {
		// Check if contact phone number meets input criteria, if not throw exception error.
		if(contactPhoneNumber.length() == 10 && checkStringIsNumeric(contactPhoneNumber)) {
			this.contactPhoneNumber = contactPhoneNumber;
		}
		else {
			throw new IllegalArgumentException("Invalid Phone Number");
		}
	}
	
	public void setContactStreetAddress(String contactStreetAddress) {
		// Check if contact street address meets input criteria, if not throw exception error.
		if (!contactStreetAddress.isBlank() && contactStreetAddress.length() <= 30) {
			this.contactStreetAddress = contactStreetAddress;
		}
		else {
			throw new IllegalArgumentException("Invalid Street Address");
		}
	}
	
	public void setContactCity(String contactCity) {
		// Check if contact city meets input criteria, if not throw exception error.
		if (!contactCity.isBlank() && contactCity.length() <= 15) {
			this.contactCity = contactCity;
		}
		else {
			throw new IllegalArgumentException("Invalid City");
		}
	}
	
	public void setContactState(String contactState) {
		// Check if contact state meets input criteria, if not throw exception error.
		if (contactState.length() == 2) {
			this.contactState = contactState;
		}
		else {
			throw new IllegalArgumentException("Invalid State");
		}
	}
	
	public void setContactZIP(String contactZIP) {
		// Check if contact ZIP meets input criteria, if not throw exception error.
		if(contactZIP.length() == 5 && checkStringIsNumeric(contactZIP)) {
			this.contactZIP = contactZIP;
		}
		else {
			throw new IllegalArgumentException("Invalid ZIP");
		}		
	}
		
	// Getter methods for the contact class.
	public String getContactID() {
		return contactID;
	}
		
	public String getContactFirstName() {
		return contactFirstName;
	}
	
	public String getContactLastName() {
		return contactLastName;
	}

	public String getContactPhoneNumber() {
		return contactPhoneNumber;
	}
	
	public String getContactStreetAddress() {
		return contactStreetAddress;
	}	
	
	public String getContactCity() {
		return contactCity;
	}	
	
	public String getContactState() {
		return contactState;
	}	

	public String getContactZIP() {
		return contactZIP;
	}	
	
	// Method checks string format. If it contains any non-numeric characters or spaces a number format exception will be thrown.
	private boolean checkStringIsNumeric(String checkString) {
		try {
			Long.parseLong(checkString);
			return true;
		} catch (NumberFormatException numberFormatException) {
			return false;
		}
	}
}
```

- ### ContactService Class.

```java
/*
 * Grand Strand System - ContactService class.
 * Developed by Jeremy Leslie 11/15/21.
 * ContactService class implements:
 *  -Contacts file import from .csv.
 *  -Contacts file export to .csv.
 *  -CRUD functionality for the Contacts class.
 */
package ContactService;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.*;

public class ContactService {
	// Declare ArrayList to store contacts.
	ArrayList<Contact> contactList = new ArrayList<>();

	// Method reads contact list from csv file.
	public void readContactsFromDisk() throws IOException {
		// Define the path to the source file.
		Path pathToFile = Paths
				.get("C:\\Users\\Keebler21084\\git\\Grand Strand System\\Grand Strand System\\contact_list.csv");
		// Create a buffer reader object to read file.
		BufferedReader bufferReader = Files.newBufferedReader(pathToFile);
		// Read the next line of the loaded file into the buffer.
		String line = bufferReader.readLine();
		// Index used to ignore the first line of the file (header).
		int index = 0;
		// Loop until next line is null.
		while (line != null) {
			// If the current line in the buffer is the header.
			if (index == 0) {
				index++;
				// Read the next line of the file into the buffer.
				line = bufferReader.readLine();
				// If the current line in the buffer is not the header.
			} else {
				// Create a string array to store parsed values from line using the comma as a
				// separator.
				String[] fileValues = line.split(",");
				// Create a new contact object using fileValues array containing parsed file
				// values.
				Contact contact = new Contact(fileValues[0].toUpperCase(), fileValues[1].toUpperCase(),
						fileValues[2].toUpperCase(), fileValues[3].toUpperCase(), fileValues[4].toUpperCase(),
						fileValues[5].toUpperCase(), fileValues[6].toUpperCase(), fileValues[7].toUpperCase());
				// Add contact to ArrayList.
				addContact(contact);
				// Read the next line of the file into the buffer.
				line = bufferReader.readLine();
			}
		}
	}

	// Method saves contact list as csv file.
	public void writeContactsToDisk() throws IOException {
		// Create a new file.
		File file = new File("contact_list.csv");
		// Create file writer and buffer writer objects to write file.
		FileWriter fileWriter = new FileWriter(file);
		BufferedWriter bufferWriter = new BufferedWriter(fileWriter);
		// Write header to file.
		bufferWriter.write("ContactID,FirstName,LastName,PhoneNumber,StreetAddress,City,State,Zip");
		// Write a new line after the header.
		bufferWriter.newLine();
		// Writer each contact in array list to file.
		for (Contact contact : contactList) {
			bufferWriter.write(contact.getContactID() + "," + contact.getContactFirstName() + ","
					+ contact.getContactLastName() + "," + contact.getContactPhoneNumber() + ","
					+ contact.getContactStreetAddress() + "," + contact.getContactCity() + ","
					+ contact.getContactState() + "," + contact.getContactZIP());
			// Write new line after each contact.
			bufferWriter.newLine();
		}
		// Close writer channels.
		bufferWriter.close();
		fileWriter.close();
	}

	// Method sorts the contact list alphabetically by last name.
	public void sortContactListByLastName() {
		contactList
				.sort((contact1, contact2) -> contact1.getContactLastName().compareTo(contact2.getContactLastName()));
	}

	// Method sorts the contact list numerically by ID number
	public void sortContactListByID() {
		contactList.sort((contact1, contact2) -> contact1.getContactID().compareTo(contact2.getContactID()));
	}

	// Method searches ArrayList for a contact using the contact Id. Returns true if
	// found, false if not found
	public boolean searchContactList(String contactID) {
		for (Contact contact : contactList) {
			if (contact.getContactID().equals(contactID)) {
				return true;
			}
		}
		return false;
	}

	// Method to add contact. Throws exception if contact Id is already in the list
	public void addContact(Contact contact) {
		if (contactList.contains(contact)) {
			throw new IllegalArgumentException("INVALID CONTACT INFO");
		} else {
			contactList.add(contact);
		}
	}

	// Method searches contact list using first name and last name and deletes
	// contact if found.
	public void deleteContact(String contactLastName, String contactFirstName) {
		Boolean contactFound = false;
		Contact contact_remove = null;
		for (Contact contact : contactList) {
			// If first name and last name match copy contact data and exit loop.
			if (contact.getContactLastName().equalsIgnoreCase(contactLastName)
					&& contact.getContactFirstName().equalsIgnoreCase(contactFirstName)) {
				contactFound = true;
				contact_remove = contact;
				break;
			}
		}
		// If the contact is found remove from list else throw exception.
		if (contactFound) {
			contactList.remove(contact_remove);
		} else {
			throw new IllegalArgumentException("CONTACT NOT FOUND");
		}
	}

	// Method updates first name of contact using contact Id. Throws exception if
	// contact Id is not found
	public void updateContactFirstName(String contactID, String updatedContactFirstName) {
		if (searchContactList(contactID)) {
			for (Contact contact : contactList) {
				if (contact.getContactID().equals(contactID)) {
					contact.setContactFirstName(updatedContactFirstName);
				}
			}
		} else {
			throw new IllegalArgumentException("INVALID ID");
		}
	}

	// Method updates last name of contact using contact Id. Throws exception if
	// contact Id is not found
	public void updateContactLastName(String contactID, String updatedContactLastName) {
		if (searchContactList(contactID)) {
			for (Contact contact : contactList) {
				if (contact.getContactID().equals(contactID)) {
					contact.setContactLastName(updatedContactLastName);
				}
			}
		} else {
			throw new IllegalArgumentException("INVALID ID 2NTACT LAST NAME");
		}
	}

	// Method updates phone # of contact using contact Id. Throws exception if
	// contact Id is not found
	public void updateContactPhoneNumber(String contactID, String updatedContactPhoneNumber) {
		if (searchContactList(contactID)) {
			for (Contact contact : contactList) {
				if (contact.getContactID().equals(contactID)) {
					contact.setContactPhoneNumber(updatedContactPhoneNumber);
				}
			}
		} else {
			throw new IllegalArgumentException("INVALID ID - CONTACT PHONE NUMBER");
		}
	}

	// Method updates street address of contact using contact Id. Throws exception
	// if contact Id is not found
	public void updateContactStreetAddress(String contactID, String updatedContactStreetAddress) {
		if (searchContactList(contactID)) {
			for (Contact contact : contactList) {
				if (contact.getContactID().equals(contactID)) {
					contact.setContactStreetAddress(updatedContactStreetAddress);
				}
			}
		} else {
			throw new IllegalArgumentException("INVALID ID - CONTACT STREET ADDRESS");
		}
	}

	// Method updates city of contact using contact Id. Throws exception if contact
	// Id is not found
	public void updateContactCity(String contactID, String updatedContactCity) {
		if (searchContactList(contactID)) {
			for (Contact contact : contactList) {
				if (contact.getContactID().equals(contactID)) {
					contact.setContactCity(updatedContactCity);
				}
			}
		} else {
			throw new IllegalArgumentException("INVALID ID - CONTACT CITY");
		}
	}

	// Method updates state of contact using contact Id. Throws exception if contact
	// Id is not found
	public void updateContactState(String contactID, String updatedContactState) {
		if (searchContactList(contactID)) {
			for (Contact contact : contactList) {
				if (contact.getContactID().equals(contactID)) {
					contact.setContactState(updatedContactState);
				}
			}
		} else {
			throw new IllegalArgumentException("INVALID ID - CONTACT STATE");
		}
	}

	// Method updates ZIP of contact using contact Id. Throws exception if contact
	// Id is not found
	public void updateContactZIP(String contactID, String updatedContactZIP) {
		if (searchContactList(contactID)) {
			for (Contact contact : contactList) {
				if (contact.getContactID().equals(contactID)) {
					contact.setContactZIP(updatedContactZIP);
				}
			}
		} else {
			throw new IllegalArgumentException("INVALID ID");
		}
	}

	// Method prints the contact list ordered alphabetically by last name.
	public void printContactListByLastName() {
		// If contact list is empty throw exception.
		if (contactList.isEmpty()) {
			throw new IllegalArgumentException("NO CONTACTS FOUND");
		}
		// If contact list is not empty sort the contact list alphabetically by last
		// name and print the contact list.
		else {			
			// Sort the contact list by last name.
			sortContactListByLastName();
			for (Contact contact : contactList) {
				System.out.println(String.format("%-12s", contact.getContactLastName()) + String.format("%-12s", contact.getContactFirstName()) + contact.getContactID());
			}
			
		}
	}

	// Method searches contact list using first name and last name and prints
	// contact info if found.
	public void printContactInfoByName(String contactFirstName, String contactLastName) {
		Boolean contactFound = false;
		for (Contact contact : contactList) {
			// If first name and last name match print contact info.
			if (contact.getContactLastName().equalsIgnoreCase(contactLastName)
					&& contact.getContactFirstName().equalsIgnoreCase(contactFirstName)) {
				System.out.println("ID: " + contact.getContactID());
				System.out.println("FIRST NAME: " + contact.getContactFirstName());
				System.out.println("LAST NAME: " + contact.getContactLastName());
				System.out.println("PHONE NUMBER: " + contact.getContactPhoneNumber());
				System.out.println("STREET ADDRESS: " + contact.getContactStreetAddress());
				System.out.println("CITY: " + contact.getContactCity());
				System.out.println("STATE: " + contact.getContactState());
				System.out.println("ZIP: " + contact.getContactZIP());
				contactFound = true;
				break;
			}
		}
		// If the contact is not found throw exception.
		if (!contactFound) {
			throw new IllegalArgumentException("CONTACT NOT FOUND");
		}
	}

	// Method returns next available contact ID number.
	public String getNextContactID() {
		// If the list contains no contacts, use the first possible ID number.
		if (contactList.isEmpty()) {
			return "1000000001";
		}
		// Else sort the list by contact ID number and add one. Convert string to
		// long for addition and back to string for return.
		else {
			sortContactListByID();
			return String.valueOf(Long.parseLong(contactList.get(contactList.size() - 1).getContactID()) + 1);
		}
	}

	// Method returns contact ID if found and string if not found.
	public String getContactID(String contactLastName, String contactFirstName) {
		for (Contact contact : contactList) {
			if (contact.getContactLastName().equalsIgnoreCase(contactLastName)
					&& contact.getContactFirstName().equalsIgnoreCase(contactFirstName)) {
				return contact.getContactID();
			}
		}
		return "ID Not Found";
	}
}
```

- ### Appointment Class.

```java
/*
 * Grand Strand System - Appointment class.
 * Developed by Jeremy Leslie 11/15/21.
 * Appointment class implements:
 * -Appointment variables
 * -Setter and getter methods for the Contacts class.
 */
package AppointmentService;

import java.util.Date;  

public class Appointment {
	// Class variables.
	String appointmentID;
	Date appointmentDate;
	String appointmentDescription;
	String appointmentFirstName;
	String appointmentLastName;

	Date currentDate = new Date();
	
	// Appointment constructor.
	public Appointment(String appointmentID, Date appointmentDate, String appointmentDescription,
			String appointmentFirstName, String appointmentLastName) {
				
		// Check if appointment id meets input criteria, if not throw exception error.
		if (appointmentID == null || appointmentID.length() != 10 || !checkStringIsNumeric(appointmentID)) {	
			throw new IllegalArgumentException("Invalid ID");
		}

		// Check if date meets input criteria, if not throw exception error.
		if (appointmentDate == null || appointmentDate.before(currentDate)) {
			throw new IllegalArgumentException("Invalid date");
		}

		// Check if appointment description meets input criteria, if not throw exception.
		if (appointmentDescription == null || appointmentDescription.length() > 50) {
			throw new IllegalArgumentException("Invalid description");
		}
		
		// Check if contact first name meets input criteria, if not throw exception.
		if (appointmentFirstName == null || appointmentFirstName.length() > 20) {
			throw new IllegalArgumentException("Invalid description");
		}
		
		// Check if contact last name meets input criteria, if not throw exception.
		if (appointmentLastName == null || appointmentLastName.length() > 20) {
			throw new IllegalArgumentException("Invalid description");
		}
		
		// Set object variables.
		this.appointmentID = appointmentID;
		this.appointmentDate = appointmentDate;
		this.appointmentDescription = appointmentDescription;
		this.appointmentFirstName = appointmentFirstName;
		this.appointmentLastName = appointmentLastName;
	}

	// Setter methods for the appointment class.
	public void setAppointmentDate(Date appointmentDate) {
		//Source the current date.
		Date date = new Date();
		// Check if date meets input criteria, if not throw exception error.
		if (appointmentDate != null && appointmentDate.after(date)) {
			this.appointmentDate = appointmentDate;
		}
		else {
			throw new IllegalArgumentException("Invalid date");
		}
	}

	public void setAppointmentDescription(String appointmentDescription) {
		// Check if appointment description meets input criteria, if not throw exception.
		if(!appointmentDescription.isBlank() && appointmentDescription.length() <= 50) {
			this.appointmentDescription = appointmentDescription;
		}
		else {
			throw new IllegalArgumentException("Invalid description");
		}
	}

	public void setAppointmentFirstName(String appointmentFirstName) {
		// Check if contact first name meets input criteria, if not throw exception.
		if (!appointmentFirstName.isEmpty() && appointmentFirstName.length() <= 20)
		this.appointmentFirstName = appointmentFirstName;
	}
	
	public void setAppointmentLastName(String appointmentLastName) {
		// Check if contact last name meets input criteria, if not throw exception.
		if (!appointmentLastName.isEmpty() && appointmentLastName.length() <= 20)
		this.appointmentLastName = appointmentLastName;
	}
	
		
	// Getter methods for the appointment class.
	public String getAppointmentID() {
		return appointmentID;
	}

	public Date getAppointmentDate() {
		return appointmentDate;
	}

	public String getAppointmentDescription() {
		return appointmentDescription;
	}
	
	public String getAppointmentFirstName() {
		return appointmentFirstName;
	}
	
	public String getAppointmentLastName() {
		return appointmentLastName;
	}
	
	// Method checks string format. If it contains any non-numeric characters or spaces a number format exception will be thrown.
	private boolean checkStringIsNumeric(String checkString) {
		try {
			Long.parseLong(checkString);
			return true;
		} catch (NumberFormatException numberFormatException) {
			return false;
		}
	}
}
```

- ### AppointmentService Class.

```java
/*
 * Grand Strand System - AppointmentService class.
 * Developed by Jeremy Leslie 11/15/21.
 * AppointmentService class implements:
 * -Appointments file import from .csv for.
 * -Appointments file export to .csv.
 * -CRUD functionality for the Appointments class.
 */
package AppointmentService;

import java.util.Date;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.ArrayList;

public class AppointmentService {
	// Declare ArrayList to store appointments.
	ArrayList<Appointment> appointmentList = new ArrayList<>();

	// Method reads appointment list from csv file.
	public void readAppointmentsFromDisk() throws IOException, ParseException {
		// Define the path to the source file.
		Path pathToFile = Paths
				.get("C:\\Users\\Keebler21084\\git\\Grand Strand System\\Grand Strand System\\appointment_list.csv");
		// Create a buffer reader object to read file.
		BufferedReader bufferReader = Files.newBufferedReader(pathToFile);
		// Read the next line of the loaded file into the buffer.
		String line = bufferReader.readLine();
		// Index used to ignore the first line of the file (header).
		int index = 0;
		// Create simple date format object to convert date to string and apply format.
		SimpleDateFormat simpleDateFormat = new SimpleDateFormat("MM/dd/yyyy");
		// Date object sources current date.
		Date appointmentDate = new Date();
		// Loop until next line is null.
		while (line != null) {
			// If the current line in the buffer is the header.
			if (index == 0) {
				index++;
				// Read the next line of the file into the buffer.
				line = bufferReader.readLine();
				// If the current line in the buffer is not the header.
			} else {
				// Create a string array to store parsed values from line using the comma as a
				// separator.
				String[] fileValues = line.split(",");
				// Convert date (string format) input from csv to date format.
				appointmentDate = simpleDateFormat.parse(fileValues[1]);
				// Create a new appointment object using fileValues array containing parsed file
				// values.
				Appointment appointment = new Appointment(fileValues[0].toUpperCase(), appointmentDate,
						fileValues[2].toUpperCase(), fileValues[3].toUpperCase(), fileValues[4].toUpperCase());
				// Add appointment to ArrayList.
				addAppointment(appointment);
				// Read the next line of the file into the buffer.
				line = bufferReader.readLine();
			}
		}
	}

	// Method saves appointment list as csv file.
	public void writeAppointmentsToDisk() throws IOException {
		// Create simple date format object to convert date to string and apply format.
		SimpleDateFormat simpleDateFormat = new SimpleDateFormat("MM/dd/yyyy");
		// String variable to hold date as string for export.
		String appointmentDateString;
		// Create a new file.
		File file = new File("appointment_list.csv");
		// Create file writer and buffer writer objects to write file.
		FileWriter fileWriter = new FileWriter(file);
		BufferedWriter bufferWriter = new BufferedWriter(fileWriter);
		// Write header to file.
		bufferWriter.write("AppointmentID,Date,Description,FirstName,LastName");
		// Write a new line after the header.
		bufferWriter.newLine();
		// Writer each appointment in array list to file.
		for (Appointment appointment : appointmentList) {
			// Get appointment date, convert to string, and apply format for export to .csv
			// file.
			appointmentDateString = simpleDateFormat.format(appointment.getAppointmentDate());
			bufferWriter.write(appointment.getAppointmentID() + "," + appointmentDateString + ","
					+ appointment.getAppointmentDescription() + "," + appointment.getAppointmentFirstName() + ","
					+ appointment.getAppointmentLastName());
			// Write new line after each contact.
			bufferWriter.newLine();
		}
		// Close writer channels.
		bufferWriter.close();
		fileWriter.close();
	}

	// Method sorts the appointment list by date.
	public void sortAppointmentListByDate() {
		appointmentList.sort((appointment1, appointment2) -> appointment1.getAppointmentDate()
				.compareTo(appointment2.getAppointmentDate()));
	}

	// Method sorts the appointment list alphabetically by last name.
	public void sortAppointmentListByLastName() {
		appointmentList.sort((appointment1, appointment2) -> appointment1.getAppointmentLastName()
				.compareTo(appointment2.getAppointmentLastName()));
	}

	// Method sorts the appointment list numerically by ID number
	public void sortAppointmentListByID() {
		appointmentList.sort((appointment1, appointment2) -> appointment1.getAppointmentID()
				.compareTo(appointment2.getAppointmentID()));
	}

	// Method searches ArrayList for an appointment using the appointment Id.
	// Returns true if found false if not
	public boolean searchList(String appointmentID) {
		for (Appointment appointment : appointmentList) {
			if (appointment.getAppointmentID().equals(appointmentID)) {
				return true;
			}
		}
		return false;
	}

	// Method to add appointment. Throws exception if appointment Id is already in
	// the list.
	public void addAppointment(Appointment appointment) {
		if (appointmentList.contains(appointment)) {
			throw new IllegalArgumentException("Invalid appointment info");
		} else {
			appointmentList.add(appointment);
		}
	}

	// Method to delete appointment. Throws exception if appointment Id is not
	// found.
	public void deleteAppointment(String appointmentID) {
		Boolean appointmentFound = false;
		Appointment temporaryAppointmentData = null;
		for (Appointment appointment : appointmentList) {
			// If appointment id matches copy appointment data and exit loop.
			if (appointment.getAppointmentID().equalsIgnoreCase(appointmentID)) {
				appointmentFound = true;
				temporaryAppointmentData = appointment;
				break;
			}
		}
		// If the appointment is found remove from list else throw exception.
		if (appointmentFound) {
			appointmentList.remove(temporaryAppointmentData);
		} else {
			throw new IllegalArgumentException("APPOINTMENT NOT FOUND");
		}
	}

	// Method to update appointment date. Throws exception if appointment Id is not
	// found.
	public void updateAppointmentDate(String appointmentID, Date appointmentDate) {
		if (searchList(appointmentID)) {
			for (Appointment appointment : appointmentList) {
				if (appointment.getAppointmentID().equals(appointmentID)) {
					appointment.setAppointmentDate(appointmentDate);
				}
			}
		} else {
			throw new IllegalArgumentException("Invalid ID");
		}
	}

	// Method to update appointment description. Throws exception if appointment Id
	// is not found.
	public void updateAppointmentDescription(String appointmentID, String appointmentDescription) {
		if (searchList(appointmentID)) {
			for (Appointment appointment : appointmentList) {
				if (appointment.getAppointmentID().equals(appointmentID)) {
					appointment.setAppointmentDescription(appointmentDescription);
				}
			}
		} else {
			throw new IllegalArgumentException("Invalid ID");
		}
	}

	// Method prints the appointment list for provided contact.
	public void printAppointmentList(String contactFirstName, String contactLastName) {
		// Index used to determine if no appointments exist for input contact.
		int index = 0;
		// If appointment list is empty throw exception.
		if (appointmentList.isEmpty()) {
			throw new IllegalArgumentException("NO APPOINTMENTS FOUND");
		}
		// If appointment list is not empty sort the appointment list alphabetically by
		// date and print the appointments that match the provided contact.
		else {
			// Create simple date format object to convert date to string and apply format.
			SimpleDateFormat simpleDateFormat = new SimpleDateFormat("MM/dd/yyyy");
			// String variable to hold date as string for printing in desired format.
			String timeString;
			// Sort the appointment list by date.
			sortAppointmentListByDate();
			for (Appointment appointment : appointmentList) {
				timeString = simpleDateFormat.format(appointment.getAppointmentDate());
				if (appointment.getAppointmentLastName().equalsIgnoreCase(contactLastName)
						&& appointment.getAppointmentFirstName().equalsIgnoreCase(contactFirstName)) {
					System.out.println(String.format("%-12s", appointment.getAppointmentID()) + " " + String.format("%-13s", timeString) +
							appointment.getAppointmentDescription());
					index++;
				}
			}
			// If no matching appointments found for input contact print message.
			if (index == 0) {
				System.out.println("NO APPOINTMENTS FOR " + contactLastName + ", " + contactFirstName + " FOUND");
			}
		}
	}

	// Method returns next available appointment ID number.
	public String getNextAppointmentID() {
		// If the list contains no appointments, use the first possible ID number.
		if (appointmentList.isEmpty()) {
			return "1000000001";
		}
		// Else sort the list by appointment ID number and add one. Convert string to
		// long for addition and back to string for return.
		else {
			sortAppointmentListByID();
			return String
					.valueOf(Long.parseLong(appointmentList.get(appointmentList.size() - 1).getAppointmentID()) + 1);
		}
	}
}
```



# ***Application Summary/Introduction - Encryption Application.***

The following c++ source code is for a simple XOR cipher application that reads an encrypted string from a file, decrypts the string, and checks its values against a user input for authorization. I have included it in this ePortfolio to demonstrate the following skills: data encryption techniques, string parsing, file read/write algorithms, simple data structure implementation, and evaluation statements.


## Execution Screen Shot(s).
![](Images/Encryption%20Program%20Execution.png)

## Source Code.

- ### Main.

```c++
/*
User validation using XOR encryption.
Imports user name, password, and user data from text file.
Decrypts import data, parses into vector, then compares against user inputs to authenticate user.
*/

#include <string>
#include <fstream>
#include <iostream>
#include <sstream>
#include <vector>
#include <conio.h>

using namespace std;

// Method shifts input string by key value to either encrypt or decrypt string data.
string encrypt_decrypt(string& input_data, string& key) {
    // Characater array to store key value.
    char key_character[2];
    // If key size matches expected value perform shift operation.
    if (key.size() == 1) {
        if (input_data.size() >= 1) {
            // Copy string to character array for shift operation.
            strcpy_s(key_character, key.c_str());;
            // Create a copy of the input string.
            string shifted_data = input_data;
            // Loop through each character in input string copying shifted value to the return string at the same index.
            for (int i = 0; i < input_data.size(); i++) {
                shifted_data[i] = input_data[i] ^ key_character[0];
            }
            return shifted_data;
        }
        else{
            cout << "INVALID USER DATA" << endl;
            return "";
        }
    }
    // If the key size doesn't match the expected value print message and return null string
    else {
        cout << "INVALID KEY" << endl;
        return "";
    }
}

// Method reads file from disk using ifstream.
string read_file(const string& file_name) {
    string return_text;

    try {
        ifstream file_input(file_name, ios::in);
        if (file_input.is_open()) {
            string line;
            // Loop through each line of file and append each line to return string.
            while (getline(file_input, line)) {
                return_text.append(line);
            }
            // Close the file.
            file_input.close();
        }
        else {
            // If the file fails to open print message.
            cout << file_name << " LOAD FAILED" << endl;
            return "";
        }
        return return_text;
    }
    catch (std::ifstream::failure e) {
        std::cerr << "Exception opening/reading/closing file\n";
    }
}

// Method takes string input and parses into vector using comma separator.
vector<string> parsed_data(string& data_file) {

    stringstream stringstream(data_file);
    vector<string> user_vector;
    string sub_string;

    // While the stringstream hasn't found new line or end of file.
    while (stringstream.good()) {
        // Get the next substring from stringstream using comma separator and push to vector.
        getline(stringstream, sub_string, ',');
        user_vector.push_back(sub_string);
    }
    return user_vector;
}

// Method checks vector elements against user inputs to authenticate user.
int user_authentication(vector<string> user_data, string user_name, string password) {
    // If first element of vector (user name) matches user name input.
    if (user_data[0].compare(user_name) == 0) {
        // If the second element of vector (password) matches user password input.
        if (user_data[1].compare(password) == 0) {
            return 0;
        }
        else {
            return -1;
        }
    }
    else {
        return -1;
    }
}

int main(int argc, const char* argv[]) {

    string user_name;
    string password;
    char password_array[20]{};
    int i = 0;
    char input_character;
    cout << "ENTER USER NAME: ";
    cin >> user_name;
    cout << "ENTER PASSWORD: ";

    // Obscure user input for password. Check user input until length equals 20 or enter key is read.
    for (int i = 0; i < 20; i++) {
        input_character = _getch();
        if (input_character == '\r') {
            break;
        }
        // If user input is not enter key add input character to password array.
        else {
            password_array[i] = input_character;
            cout << "*";
            password += password_array[i];
        }
    }
    cout << endl;

    // Reads the key value from file.
    std::string key_string = read_file("key.txt");
 
    // Read encrypted data file.
    string encrypted_data = read_file("encrypted_data_file.txt");
    // decrypt data file.
    string decrypted_data = encrypt_decrypt(encrypted_data, key_string);

    // Parse input data using comma separater and store as vector.
    vector<string> user_vector = parsed_data(decrypted_data);
    // If vector size equals the expected value.
    if (user_vector.size() == 3) {
        // Check user login credentials.
        if (user_authentication(user_vector, user_name, password) == 0) {
            cout << "AUTHENTICATION SUCCESSFUL" << endl;
            // If both data files succeeded print data.
            cout << "ENCRYPTED DATA: " + encrypted_data << endl;
            // Output to show decrypted string.
            cout << "DECRYPTED DATA: " + decrypted_data << endl;
            // TODO. RETURN VALUE TO CALLING PROCESS CONFIRMING USER AUTHENTICATION.
        }
        // If authentication fails print message.
        else {
            cout << "AUTHENTICATION FAILED" << endl;
            // TODO. RETURN VALUE TO CALLING PROCESS DENYING USER AUTHENTICATION.
        }
    }
    return 0;
}
```



# ***Application Summary/Introduction - MongoDB with JavaScript Database Management Application.***

The following source code is for a database management application that operates on the machine local host. The server is implemented using node with express, the MongoDB database is implemented using the mongoose driver, and the views are coded in html. I have included this application in this ePortfolio to demonstrate the following skills: database implementation, simple server implementation, and user interface design.

## Execution Screen Shot(s).
  - ### Animal List View.
  ![](Images/Animal%20List%20Print%20Screen.png)
  - ### Insert View.
  ![](Images/Insert%20Animal%20Screen.png)
  - ### Update View.
  ![](Images/Update%20Animal%20Screen.png)
  - ### Delete View.
  ![](Images/Delete%20Prompt.png)
  - ### Find View.
  ![](Images/Find%20Animal%20Screen.png)
  ![](Images/Query%20List%20Screen.png)
  
## Source Code.
 
- ### Animal Controller (JavaScript).

```js
/*
Animal controller.
Main control class for MongoDB CRUD operations.
Uses express web application framework.
*/

// Import express module.
const express = require('express')
// Create express router object to handle database requests.
var router = express.Router()
// Import mongodb mongoose module.
const mongoose = require('mongoose')
// Create animal schema wrapper.
const Animal = mongoose.model('Animal')

// Route handler executed when the create new button is clicked from the animal list view.
// Renders the animal addOrEdit view and sets view title.
router.get('/', (req, res)=>{
    // Render the addOrEdit view.
    res.render('animal/addOrEdit', {
        // Set the view title.
        viewTitle: 'Insert Animal'
    })
})

// Route handler executed when findByID button is clicked from the animal list view. 
// Renders the animal find view and sets view title.
router.get('/find', (req, res)=>{
    // Render the find view.
    res.render('animal/find', {
        // Set the view title.
        viewTitle: 'Find Animal By ID'
    })
})

// Route handler executed when submit button is clicked from the find view. 
// Queries database using animal_id, renders the animal list view, and prints the returned documents.
router.get('/findByID', (req, res) => {
    // Query database for matching entries using animal_id.
    Animal.find({animal_id : req.body.animal_id}, (err, doc) => {
        // If query succeeds render list view and print returned documents.
        if (!err) {
            res.render('animal/list', {
                list: doc
            });
        }
        // If query fails print error message to console.
        else {
            
            console.log(doc);
        }
    });
});

// Route handler executes when the submit button is clicked from the addOrEdit view.
router.post('/', (req, res) => {
    // If the id is null create a new record.
    if (req.body._id == '') {
        insertRecord(req,res)
    }
    // If the id is not null update record matching id.
    else {
        updateRecord(req,res)
    }
})

// Function to insert a new record.
function insertRecord(req, res) {
    // Create new animal object.
    var animal = new Animal()
    // Set animal variables.
    animal.animal_id = req.body.animal_id;
    animal.animal_type = req.body.animal_type;
    animal.age_upon_outcome = req.body.age_upon_outcome;
    animal.breed = req.body.breed;
    // Save database.
    animal.save((err, doc) => {
        // If save succeeds redirect route handler path to /list.
        if (!err) {
            res.redirect('animal/list')
        }
        // If save fails print error message to console.
        else {
            console.log('Error during insert: ' + err)
        }
    })
}

// Function to update a record.
function updateRecord(req,res) {
    // Query database for mathcing entry using id.
    Animal.findOneAndUpdate({_id: req.body._id},
        req.body,
        {new: true},
        (err, doc) => {
            // If update succeeds redirect route handler path to /list.
            if (!err) {
                res.redirect('animal/list')
            }
            // If update fails print error message to console.
            else {
                console.log('Error during update: ' + err);
            }
        }
    );
}

// Route handler executes when the view all button is clicked from any view.
router.get('/list', (req, res) => {
    // Query database for all entries.
    Animal.find((err, docs) => {
        // If query succeeds render the animal list view.
        if (!err) {
            res.render('animal/list', {
                list: docs
            })
        }
        // If query fails print error message to console.
        else {
            console.log('Error in retrieval: ' + err)
        }
    });
});

// Route handler executes when the edit button is clicked from the list view.
router.get('/:id', (req, res) => {
    // Query database for matching entry using ID.
    Animal.findById(req.params.id, (err, doc) => {
        // If query is successful render the addOrEdit view, set the view title, and populate form data.
        if(!err) {
            res.render('animal/addOrEdit', {
                viewTitle: 'Update Animal',
                animal: doc,
            });
        }
        // If query fails print error message to console.
        else {
            console.log(doc);
        }
    });
});

// Find animal by id and remove from list.
router.get('/delete/:id', (req,res) => {
    // Query database for matching entry using ID and remove any matches.
    Animal.findByIdAndRemove(req.params.id, (err, doc) => {
        // If query is successful find all database entries.
        if (!err) {
            // Query database for all entries.
            Animal.find((err, docs) => {
                // If query is successful render the list view, and print all documents.
                if (!err) {
                    res.render('animal/list', {
                        list: docs
                    })
                }
                // If query fails print error message to console.
                else {
                    console.log('Error in retrieval: ' + err)
                }
            });
        }
        // If query fails print error message to console.
        else {
            console.log('Error in deletion: ' + err);
        }
    });
});

module.exports = router
```

- ### Animal Model (JavaScript).

```js
/*
Animal object database schema.
*/

// Import mongodb mongoose module.
const mongoose = require('mongoose')

// Create new schema object and define object variables.
var animalSchema = new mongoose.Schema({
    animal_id: {
        type: String,
        require: 'This field is required'
    },
    animal_type: {
        type: String,
        required: 'This field is required'
    },
    age_upon_outcome: {
        type: String,
        require: 'This field is required'
    },
    breed: {
        type: String,
        required: 'This field is required'
    },
})

mongoose.model('Animal',animalSchema);
```

- ### Database Controller (JavaScript).

```js
/*
Establish connection to database using mongoose driver.
*/

// Import mongodb mongoose library.
const mongoose = require('mongoose')

// Set the connection path for the mongoose object.
mongoose.connect("mongodb://localhost:27017/aac_shelter_outcomes", {
    useNewUrlParser: true
},
err=> {
    // If connection succeeds print success message to console.
    if (!err){
        console.log('Connection successful')
    }
    // If connection fails print error message to console.
    else {
        console.log('Error in connection' + err)
    }
})

require('./animal.model')
```

- ### Index (JavaScript).

```js
/*
Establishes connection path.
Loads external libraries express, handlebars, and express handlebars.
Opens socket on local host port 3000.
*/
require('./models/db')

// Import express module.
const express = require('express');
// Create path object for IP.
const path = require("path");
// Import handlebars module.
const handlebars = require("handlebars");
// Import express handlebars module.
const {engine} = require("express-handlebars");
const {allowInsecurePrototypeAccess} = require("@handlebars/allow-prototype-access");
const bodyparser = require("body-parser");
// Import controller module.
const animalController = require('./controllers/animalController');

// Create express object for app framework.
var app = express();
app.use(bodyparser.urlencoded({extended: false}));
app.use(bodyparser.json());

// Set the path to callback.
app.get('/', (req, res) => {
    res.send('<h2>Welcome to Animals Database!!</h2><h3>Click here to get access to the <b> <a href="/animal/list">Database</a></b></h3>');
});

// set the express path name.
app.set("views", path.join(__dirname,"/views/"));

// Initialize the app framework and set the view.
app.engine('hbs', engine({
    handlebars: allowInsecurePrototypeAccess(handlebars),
    extname: 'hbs',
    defaultLayout: "MainLayout",
    layoutsDir: __dirname + "/views/layouts/"
}));

// Set the view engine to handlebars.
app.set("view engine", "hbs");
// Initatie listening on port 3000.
app.listen(3000, () => {
    console.log("Express server started at port 3000");
});

app.use('/animal', animalController);
```

- ### Add Or Edit View (html).

```html
<!--view model for animal database add or edit-->

<!-- Header-->
<h3>{{viewTitle}}</h3>

<!-- Form settings-->
<form action='/animal' method="POST" autocomplete="off">

    <!--Input fields for animal variables-->
    <input type="hidden" name="_id" value="{{animal._id}}"> 
    <div class="form-group">
        <label>ID</label>
        <input type="text" class="form-control" name="animal_id" placeholder="ID" value="{{animal.animal_id}}">
    </div>
        <div class="form-group">
        <label>Type</label>
        <input type="text" class="form-control" name="animal_type" placeholder="Type" value="{{animal.animal_type}}">
    </div>
    <div class="form-group">
        <label>Age</label>
        <input type="text" class="form-control" name="age_upon_outcome" placeholder="Age" value="{{animal.age_upon_outcome}}">
    </div>
    <div class="form-group">
        <label>Breed</label>
        <input type="text" class="form-control" name="breed" placeholder="Breed" value="{{animal.breed}}">
    </div>    

    <!--User interface buttons-->
    <div class="form-group">
        <button type="submit" class="btn btn-info">Submit</button>
        <a class="btn btn-secondary" href="/animal/list">View All</a>    
    </div>
</form>
```

- ### Find View (html).

```html
<!--view model for animal database add or edit-->

<!-- Header-->
<h3>{{viewTitle}}</h3>

<!-- Form settings-->
<form action='/animal' method="POST" autocomplete="off">

    <!--Input fields for animal variables-->
    <input type="hidden" name="_id" value="{{animal._id}}">
    <div class="form-group">
        <label>ID</label>
        <input type="text" class="form-control" name="animal_id" placeholder="ID" value="{{animal.animal_id}}">
    </div>
    
    <!--User interface buttons-->
    <div class="form-group">
        <a class ="btn btn-primary" href="/animal/findByID">Submit</a>
        <a class="btn btn-secondary" href="/animal/list">View All</a>    
    </div>
</form>
```

- ### List View (html).

```html
{{!-- 
view model for animal database print list
--}}

<!-- Header including user interface buttons-->
<h3> Animal List 
    <a class="btn btn-secondary" href="/animal"> Create New </a>
    <a class="btn btn-secondary" href="/animal/find"> Find By ID </a>
</h3>

<!-- Table to display database data-->
<table class="table table-striped">
    <!-- Table headers-->
    <thead>
        <tr>
            <th>ID</th>
            <th>Type</th>
            <th>Age          </th>
            <th>Breed</th>
            <th>Color</th>
        </tr>
    </thead>

    <!-- Loop displays each entry in database-->
    <tbody>
        {{#each list}}
        <tr>
            <td>{{this.animal_id}}</td>
            <td>{{this.animal_type}}</td>
            <td>{{this.age_upon_outcome}}</td>
            <td>{{this.breed}}</td>
            <td>
                <!-- User interface buttons for edit and delete-->
                <a class="btn btn-primary btn-sm" href="/animal/{{this.id}}">Edit</a>
                <a class="btn btn-primary btn-sm" href="/animal/delete/{{this.id}}"
                onclick="return confirm('Are you sure you want to delete this record?');">Delete</a>
            </td>
        </tr>
        {{/each}}
    </tbody>
</table>
```

- ### Main Layout Scheme (html).

```html
<!--view model for main page layout and color schemes.-->

<!DOCTYPE html>
<html>

<head>
    <title>Animal</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
    <style>
        #box {
        background-color: #fff;
        margin-top: 25px;
        padding: 20px;
        -webkit-box-shadow: 10px 10px 20px 1px rgba(0,0,0,0.75);
        -moz-box-shadow: 10px 10px 20px 1px rgba(0,0,0,0.75);
        box-shadow: 10px 10px 20px 1px rgba(0,0,0,0.75);
        border-radius: 10px 10px 10px 10px;
        -moz-border-radius: 10px 10px 10px 10px;
        -webkit-border-radius: 10px 10px 10px 10px;
        border: 0px solid #000000;
        }
    </style>
</head>

<body class='bg-info'>
    <div id='box' class="col-md-6 offset-md-3">
        {{{body}}}
    </div>
</body>

</html>
```
