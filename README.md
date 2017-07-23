# IssueManagementSystem

package IssueManagement;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;
import java.util.Scanner;

public class ManagementSystem {

    static ArrayList<Issue> TheIssueList;

    public static void main(String[] args) {

        TheIssueList = new ArrayList<>();

        System.out.println("Welcome to Issue Management System!");

        for (;;) {
            MainInterface();

            Scanner scnea = new Scanner(System.in);
            String scanEA = scnea.next();

            if ("E".equals(scanEA)) {
                AddingIssueByEmployee();

                System.out.println("Quit program?");

                boolean quit = scnea.nextBoolean();

                if (quit) {
                    break;
                }
            } else if ("A".equals(scanEA)) {
                AdminStaffMenu();
            } else {
                System.out.println("Invalid input, try again!");
            }

        }

    }
    public static void MainInterface() {
        System.out.println("Please type 'E' if you're an Employee, or 'A' if "
                + "you are an Administrative Staff.");
    }
    public static void AddingIssueByEmployee() {

        Issue problem = new Issue();

        Scanner scninpt = new Scanner(System.in);

        System.out.println("Please enter details of the Issue below.");

        System.out.println("Title: ");
        problem.Title = scninpt.nextLine();

        System.out.println("Description: ");
        problem.Description = scninpt.nextLine();

        System.out.println("Department Code: ");
        problem.DepartmentCode = scninpt.nextLine();

///////////////////////////////////////////////////////////////////
        boolean dateFormat;
        
        do {
            System.out.println("Today's Date (dd/MM/yy): ");
            String tempDate = scninpt.nextLine();

            SimpleDateFormat date = new SimpleDateFormat("dd/MM/yyyy");

            try {
                problem.DateReported = date.parse(tempDate);  ///// STRING - DATE
                dateFormat = true;
            } catch (ParseException p) {
                dateFormat = false;
                System.out.println("Invalid format, try again!");
            }
        } while (!dateFormat);

///////////////////////////////////////////////////////////////////////
        boolean deadlineFormat;

        do {
            System.out.println("Deadline (dd/MM/yy): ");
            String tempDeadline = scninpt.nextLine();
            SimpleDateFormat deadline = new SimpleDateFormat("dd/MM/yyyy");

            try {
                problem.Deadline = deadline.parse(tempDeadline);
                deadlineFormat = true;
            } catch (ParseException p) {
                deadlineFormat = false;
                System.out.println("Invalid format, try again.");
            }
        } while (!deadlineFormat);

///////////////////////////////////////////////////////////////////////
        System.out.println("Reported By: ");
        problem.ReportedBy = scninpt.nextLine();

        System.out.println("Do you want to add this issue to the list?");

        boolean decided = scninpt.nextBoolean();

        if (decided) {
            TheIssueList.add(problem);
            System.out.println("Issue ADDED to the list!\n");
        } else {
            System.out.println("Issue NOT ADDED in the list!\n");
        }
        
    }
    public static void AdminStaffMenu() {

        for (;;) {
            Interface2();

            Scanner ums = new Scanner(System.in);
            int umss = ums.nextInt();

            if (umss == 1) {
                IssueListSelect();
                break;
            } else if (umss == 2) {
                SearchSelect();
                break;
            } else if (umss == 3) {
                EditSelect();
                break;
            } else {
                System.out.println("Invalid value entered!");
            }

        }

    }
    public static void IssueListSelect() {

        for (Issue prblm : TheIssueList) {

            System.out.println("----------------------");
            System.out.println("Title: " + prblm.Title);
            System.out.println("Description: " + prblm.Description);
            System.out.println("Department Code: " + prblm.DepartmentCode);
            System.out.println("Date Entered: " + prblm.DateReported);
            System.out.println("Deadline: " + prblm.Deadline);
            System.out.println("Reported By: " + prblm.ReportedBy);
            System.out.println("--- Administrative action ---");
            System.out.println("Comment: " + prblm.Comment);
            System.out.println("Current Status: " + prblm.Status);
            System.out.println("Assigned To: " + prblm.AssignedTo);

            System.out.println("----------------------");
        }
    }
    public static void SearchSelect() {
///////////////////////////////////////////////////////////////////////////////
        Date deadline = null;
        boolean DeadlineFormat1;
        
        do {
            System.out.println("Please enter a deadline (dd/MM/yyyy): ");
            Scanner scnddln = new Scanner(System.in);
            String deadlineString = scnddln.nextLine();

            try {
                deadline = new SimpleDateFormat("dd/MM/yyyy").parse(deadlineString);   // STRING -> DATE
                DeadlineFormat1 = true;
            } catch (ParseException e) {
                System.out.println("Hey its wrong!");
                DeadlineFormat1 = false;
            }
        } while (!DeadlineFormat1);
////////////////////////////////////////////////////////////////////////////////
        for (Issue d : TheIssueList) {
            if (deadline.equals(d.Deadline)) {    /// COMPARING DATE TO DATE HERE.
                System.out.println("----- Issue Info -----");
                DetailPrinting(d);
            } else {
                System.out.println("No matching deadline found!");
            }
        }
    }
    public static void EditSelect() {
        boolean deadlineFormat2;
        Date deadline2 = null;
        do {
            System.out.println("Please enter the deadline(dd/MM/yyyy) of an issue to Edit for: ");
            Scanner dll = new Scanner(System.in);
            String dl_string = dll.nextLine();
            try {
                deadline2 = new SimpleDateFormat("dd/MM/yyyy").parse(dl_string);
                deadlineFormat2 = true;
                System.out.println("That's correct!!");
            } catch (ParseException e) {
                System.out.println("Heyy you entered in a wrong format!");
                deadlineFormat2 = false;
            }
        } while (!deadlineFormat2);

        for (Issue d : TheIssueList) {

            if (deadline2.equals(d.Deadline)) {
                System.out.println("Do you want to add Comment, Status or AssignTo? ");
                System.out.println("1, for Comment \n2, for Status \n3, To Assign\n");

                Scanner scnedtt = new Scanner(System.in);
                int scnedt = scnedtt.nextInt();

                switch (scnedt) {
                    case 1:
                        System.out.println("Please add your comment below:");
                        Scanner scncmnt = new Scanner(System.in);

                        d.Comment = scncmnt.nextLine();
                        System.out.println("Comment Added!");
                        break;

                    case 2:
                        System.out.println("Please add status below:");
                        Scanner scnstts = new Scanner(System.in);

                        d.Status = scnstts.nextLine();
                        System.out.println("Status Updated!");
                        break;

                    case 3:
                        System.out.println("Who do you assign To? ");
                        Scanner scnassgn = new Scanner(System.in);

                        String responsible = d.AssignedTo = scnassgn.nextLine();

                        System.out.println("The Job has been Assigned to " + responsible);
                        break;

                    default:
                        System.out.println("The value entered is invalid!");
                }

            } else {
                System.out.println("Couldn't find an issue with this deadline!");
            }
        }
    }
    public static void Interface2() {
        System.out.println("Please type:");
        System.out.println("1, for IssueList \n2, for Search \n3, for Edit\n");
    }
    public static void DetailPrinting(Issue i) {

        System.out.println("----------------------");
        System.out.println("Title: " + i.Title);
        System.out.println("Description: " + i.Description);
        System.out.println("Department Code: " + i.DepartmentCode);
        System.out.println("Date Entered: " + i.DateReported);
        System.out.println("Deadline: " + i.Deadline);
        System.out.println("Reported By: " + i.ReportedBy);
        System.out.println("--- Administrative action ---");
        System.out.println("Comment: " + i.Comment);
        System.out.println("Current Status: " + i.Status);
        System.out.println("Assigned To: " + i.AssignedTo);
        System.out.println("----------------------");
    }
}

//////////////// Code ends here ////////////////////////////
/// new Issue.java class is as follows////

package IssueManagement;

import java.util.Date;

public class Issue {
   
    String Title;
    String Description;
    String DepartmentCode;
    Date DateReported;
    Date Deadline;
    String ReportedBy;
    
    String Status;
    String Comment;
    String AssignedTo;
}
