# JavaCrud-
# Java-Crud-
DBConnection.java

import java.sql.Connection;
import java.sql.DriverManager;
public class DBConnection {

    public static Connection getConnection() {

        Connection con = null;

        try {

            Class.forName("com.mysql.cj.jdbc.Driver");

            con = DriverManager.getConnection(
                "jdbc:mysql://localhost:3306/college",
                "root",
                "@Shiva123"
            );

            System.out.println("Database Connected!");

        } catch(Exception e){
            e.printStackTrace();
        }

        return con;
    }
}

Main.java

import java.util.Scanner;

public class Main {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        StudentDAO dao = new StudentDAO();

        while(true){

            System.out.println("\n===== STUDENT CRUD MENU =====");
            System.out.println("1. Insert Student");
            System.out.println("2. View Students");
            System.out.println("3. Update Student");
            System.out.println("4. Delete Student");
            System.out.println("5. Exit");

            System.out.print("Enter Choice: ");
            int choice = sc.nextInt();

            switch(choice){

                case 1:
                    dao.insertStudent();
                    break;

                case 2:
                    dao.viewStudents();
                    break;

                case 3:
                    dao.updateStudent();
                    break;

                case 4:
                    dao.deleteStudent();
                    break;

                case 5:
                    sc.close();
                    System.exit(0);

                default:
                    System.out.println("Invalid Choice");
            }
        }
    }
}

StudentDAO.java

import java.sql.*;
import java.util.Scanner;

public class StudentDAO {

    Scanner sc = new Scanner(System.in);

    // CREATE
    public void insertStudent() {

        try {
            Connection con = DBConnection.getConnection();

            System.out.print("Enter Name: ");
            String name = sc.next();

            System.out.print("Enter Age: ");
            int age = sc.nextInt();

            System.out.print("Enter Course: ");
            String course = sc.next();

            String query =
                    "INSERT INTO student(name,age,course) VALUES(?,?,?)";

            PreparedStatement ps = con.prepareStatement(query);

            ps.setString(1,name);
            ps.setInt(2,age);
            ps.setString(3,course);

            ps.executeUpdate();

            System.out.println("Student Added Successfully!");

        } catch(Exception e){
            e.printStackTrace();
        }
    }

    // READ
    public void viewStudents() {

        try {
            Connection con = DBConnection.getConnection();

            Statement st = con.createStatement();

            ResultSet rs = st.executeQuery("SELECT * FROM student");

            System.out.println("\nID  Name  Age  Course");

            while(rs.next()) {

                System.out.println(
                        rs.getInt("id")+"  "+
                        rs.getString("name")+"  "+
                        rs.getInt("age")+"  "+
                        rs.getString("course")
                );
            }

        } catch(Exception e){
            e.printStackTrace();
        }
    }

    // UPDATE
    public void updateStudent() {

        try {
            Connection con = DBConnection.getConnection();

            System.out.print("Enter Student ID: ");
            int id = sc.nextInt();

            System.out.print("Enter New Course: ");
            String course = sc.next();

            String query =
                    "UPDATE student SET course=? WHERE id=?";

            PreparedStatement ps = con.prepareStatement(query);

            ps.setString(1,course);
            ps.setInt(2,id);

            ps.executeUpdate();

            System.out.println("Updated Successfully!");

        } catch(Exception e){
            e.printStackTrace();
        }
    }

    // DELETE
    public void deleteStudent() {

        try {
            Connection con = DBConnection.getConnection();

            System.out.print("Enter Student ID: ");
            int id = sc.nextInt();

            String query =
                    "DELETE FROM student WHERE id=?";

            PreparedStatement ps = con.prepareStatement(query);

            ps.setInt(1,id);

            ps.executeUpdate();

            System.out.println("Deleted Successfully!");

        } catch(Exception e){
            e.printStackTrace();
        }
    }
}
