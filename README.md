# Pet-Adoption-Portal
package PetAdopt;

import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

public class PetAdoptionPortal extends JFrame implements ActionListener {

    PetAdoptionPortal() {
        ImageIcon i1 = new ImageIcon(ClassLoader.getSystemResource("icons/logo.jpg"));

        // Adjusted size for the JFrame
        setSize(1100, 660); // Increased size
        setLocationRelativeTo(null); // Center the window
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        JPanel panel = new JPanel() {
            @Override
            protected void paintComponent(Graphics g) {
                super.paintComponent(g);
                // Calculate the scale to fit the image within the panel
                double scaleWidth = (double) getWidth() / i1.getIconWidth();
                double scaleHeight = (double) getHeight() / i1.getIconHeight();
                double scale = Math.min(scaleWidth, scaleHeight);
                int newWidth = (int) (i1.getIconWidth() * scale);
                int newHeight = (int) (i1.getIconHeight() * scale);
                // Draw the image centered within the panel
                int x = (getWidth() - newWidth) / 2;
                int y = (getHeight() - newHeight) / 2;
                g.drawImage(i1.getImage(), x, y, newWidth, newHeight, this);
            }
        };
        setContentPane(panel);

        JMenuBar mb = new JMenuBar();
        setJMenuBar(mb); // Add the JMenuBar to the JFrame

        JMenu home = new JMenu("HOME PAGE");
        JMenu login = new JMenu("SIGN UP/SIGN IN");

        JMenuItem register = new JMenuItem("Register");
        register.setForeground(Color.BLUE);
        register.setActionCommand("Register");
        register.addActionListener(this);

        JMenuItem l = new JMenuItem("Login");
        l.setForeground(Color.BLUE);
        l.setActionCommand("Login");
        l.addActionListener(this);

        JMenu adopt = new JMenu("ADOPTION");
        JMenuItem pets = new JMenuItem("View Pets");
        pets.setForeground(Color.BLUE);
        pets.addActionListener(this);

        JMenu help = new JMenu("HELP CENTRE");
        JMenuItem he = new JMenuItem("Help");
        he.setForeground(Color.BLUE);
        he.setActionCommand("Help"); // Added action command for help menu item
        he.addActionListener(this);

        // Menu bar
        mb.add(home);
        mb.add(login);
        mb.add(adopt);
        mb.add(help);

        // Menu items
        login.add(register);
        login.add(l);
        adopt.add(pets);
        help.add(he);

        setVisible(true);
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            new PetAdoptionPortal();
        });
    }

    @Override
    public void actionPerformed(ActionEvent ae) {
        String command = ae.getActionCommand();
        if (command.equals("Login")) {
            new ALogin();
        } else if (command.equals("Register")) {
            new Reg();
        } else if (command.equals("View Pets")) {
            new Pet();
        }
        else if (command.equals("Help")) {
            new Help();
        }
    }
}
package PetAdopt;

import javax.swing.*;
import javax.swing.table.DefaultTableModel;
import java.awt.*;
import java.sql.*;

public class Pet extends JFrame {
    Pet() {
        setTitle("Available Pets");
        setSize(800, 600); // Increased window size
        setLocationRelativeTo(null);
        setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);

        // Create a panel to hold components
        JPanel panel = new JPanel(new BorderLayout());

        // Create a table model with column names
        DefaultTableModel tableModel = new DefaultTableModel(new String[]{"Pet ID", "Species", "Breed", "Gender", "Age"}, 0);
        JTable petTable = new JTable(tableModel);
        JScrollPane scrollPane = new JScrollPane(petTable);

        // Add the table to the panel
        panel.add(scrollPane, BorderLayout.CENTER);

        // Create a label for title
        JLabel titleLabel = new JLabel("List of Available Pets");
        titleLabel.setFont(new Font("Arial", Font.BOLD, 20));
        titleLabel.setHorizontalAlignment(SwingConstants.CENTER);
        panel.add(titleLabel, BorderLayout.NORTH);

        // Add the panel to the frame
        add(panel);

        try {
            // Establish connection
            Connection connection = DriverManager.getConnection("jdbc:oracle:thin:@//LAPTOP-K81RSSBS:1521/orcl", "system", "arya2901");

            // Prepare and execute the query to fetch all records from the pets table
            String query = "SELECT * FROM pets";
            PreparedStatement preparedStatement = connection.prepareStatement(query);
            ResultSet resultSet = preparedStatement.executeQuery();

            // Add rows to the table model
            while (resultSet.next()) {
                int petId = resultSet.getInt("pet_id");
                String species = resultSet.getString("species");
                String breed = resultSet.getString("breed");
                String gender = resultSet.getString("gender");
                int age = resultSet.getInt("age");
                tableModel.addRow(new Object[]{petId, species, breed, gender, age});
            }

            // Close resources
            resultSet.close();
            preparedStatement.close();
            connection.close();
        } catch (SQLException ex) {
            ex.printStackTrace();
            JOptionPane.showMessageDialog(this, "Error: " + ex.getMessage());
        }

        setVisible(true);
    }

    public static void main(String[] args) {
        new Pet();
    }
}
package PetAdopt;

import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class Adopt extends JFrame {

    Adopt() {
        setSize(400, 350); // Reduced height
        setLocation(600, 200); // Adjusted location
        setLayout(null);

        // Adding JLabel for "Adoption Form" text
        JLabel formLabel = new JLabel("Adoption Form");
        formLabel.setFont(new Font("Arial", Font.BOLD, 20));
        formLabel.setForeground(Color.BLUE);
        formLabel.setBounds(120, 10, 200, 30);
        add(formLabel);

        JLabel nameLabel = new JLabel("Name:");
        nameLabel.setBounds(50, 50, 100, 30);
        add(nameLabel);

        JTextField nameField = new JTextField();
        nameField.setBounds(160, 50, 150, 30);
        add(nameField);

        JLabel contactLabel = new JLabel("Contact:");
        contactLabel.setBounds(50, 90, 100, 30);
        add(contactLabel);

        JTextField contactField = new JTextField();
        contactField.setBounds(160, 90, 150, 30);
        add(contactField);

        JLabel emailLabel = new JLabel("Email:");
        emailLabel.setBounds(50, 130, 100, 30);
        add(emailLabel);

        JTextField emailField = new JTextField();
        emailField.setBounds(160, 130, 150, 30);
        add(emailField);

        JLabel addressLabel = new JLabel("Address:");
        addressLabel.setBounds(50, 170, 100, 30);
        add(addressLabel);

        JTextField addressField = new JTextField();
        addressField.setBounds(160, 170, 150, 30);
        add(addressField);

        JLabel idLabel = new JLabel("Pet ID:");
        idLabel.setBounds(50, 210, 100, 30);
        add(idLabel);

        JTextField idField = new JTextField();
        idField.setBounds(160, 210, 150, 30);
        add(idField);

        JButton submitButton = new JButton("Submit");
        submitButton.setBackground(Color.BLACK);
        submitButton.setForeground(Color.WHITE);
        submitButton.setBounds(120, 260, 100, 30);
        add(submitButton);

        JButton cancelButton = new JButton("Cancel");
        cancelButton.setBackground(Color.BLACK);
        cancelButton.setForeground(Color.WHITE);
        cancelButton.setBounds(240, 260, 100, 30);
        add(cancelButton);

        // Add action listener to the submit button
        submitButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                // Get values from text fields
                String name = nameField.getText();
                String contact = contactField.getText();
                String email = emailField.getText();
                String address = addressField.getText();
                String petIdStr = idField.getText();

                // Validate fields
                if (name.isEmpty() || contact.isEmpty() || email.isEmpty() || address.isEmpty() || petIdStr.isEmpty()) {
                    JOptionPane.showMessageDialog(Adopt.this, "Please fill in all fields.");
                    return;
                }

                // Validate contact number format
                if (!contact.matches("\\d{10}")) {
                    JOptionPane.showMessageDialog(Adopt.this, "Invalid contact number. Please enter a 10-digit number.");
                    return;
                }

                // Validate email format
                if (!email.matches("\\b[\\w.%-]+@[-.\\w]+\\.[A-Za-z]{2,4}\\b")) {
                    JOptionPane.showMessageDialog(Adopt.this, "Invalid email address format.");
                    return;
                }

                int petId;
                try {
                    petId = Integer.parseInt(petIdStr);
                } catch (NumberFormatException ex) {
                    JOptionPane.showMessageDialog(Adopt.this, "Invalid Pet ID. Please enter a valid number.");
                    return;
                }

                // Insert into the database
                if (insertIntoDatabase(name, contact, email, address, petId)) {
                    // Show success message
                    JOptionPane.showMessageDialog(Adopt.this, "Form submitted successfully");
                    // Close the window
                    dispose();
                }
            }
        });

        // Add action listener to the cancel button
        cancelButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                // Close the window
                dispose();
            }
        });

        setVisible(true);
    }

    // Method to insert data into the database
    private boolean insertIntoDatabase(String name, String contact, String email, String address, int petId) {
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null; // Declare resultSet here
        boolean petExists = false; // Flag to check if the pet ID exists
        try {
            // Establish connection
            connection = DriverManager.getConnection("jdbc:oracle:thin:@//LAPTOP-K81RSSBS:1521/orcl", "system", "arya2901");

            // Check if the pet ID exists in the pets table
            String checkPetQuery = "SELECT * FROM pets WHERE pet_id = ?";
            preparedStatement = connection.prepareStatement(checkPetQuery);
            preparedStatement.setInt(1, petId);
            resultSet = preparedStatement.executeQuery(); // Execute query and assign to resultSet

            // If the pet ID exists, set the flag to true
            if (resultSet.next()) {
                petExists = true;
            }

            // If the pet ID doesn't exist, show a pop-up and return false
            if (!petExists) {
                JOptionPane.showMessageDialog(this, "Invalid pet ID or pet is not available");
                return false;
            }

            // Prepare statement for inserting adoption form data
            String insertQuery = "INSERT INTO adoption (name, contact, email, address, petid) VALUES (?, ?, ?, ?, ?)";
            preparedStatement = connection.prepareStatement(insertQuery);
            preparedStatement.setString(1, name);
            preparedStatement.setString(2, contact);
            preparedStatement.setString(3, email);
            preparedStatement.setString(4, address);
            preparedStatement.setInt(5, petId);

            // Execute update
            int rowsAffected = preparedStatement.executeUpdate();
            if (rowsAffected > 0) {
                // Delete the record from the pets table
                String deleteQuery = "DELETE FROM pets WHERE pet_id = ?";
                preparedStatement = connection.prepareStatement(deleteQuery);
                preparedStatement.setInt(1, petId);
                preparedStatement.executeUpdate();

                return true; // Successful insertion and deletion
            } else {
                JOptionPane.showMessageDialog(this, "Failed to insert record");
                return false; // Failed insertion
            }
        } catch (SQLException ex) {
            ex.printStackTrace();
            JOptionPane.showMessageDialog(this, "Error: " + ex.getMessage());
            return false; // Failed insertion due to exception
        } finally {
            // Close resources
            if (resultSet != null) {
                try {
                    resultSet.close();
                } catch (SQLException ex) {
                    ex.printStackTrace();
                }
            }
            if (preparedStatement != null) {
                try {
                    preparedStatement.close();
                } catch (SQLException ex) {
                    ex.printStackTrace();
                }
            }
            if (connection != null) {
                try {
                    connection.close();
                } catch (SQLException ex) {
                    ex.printStackTrace();
                }
            }
        }
    }

}

package PetAdopt;

import javax.swing.*;
import java.awt.*;

public class Help extends JFrame {

    Help() {
        setTitle("Help");
        setSize(800, 600); // Increased window size
        setLocationRelativeTo(null); // Center the window
        setLayout(new BorderLayout());

        JPanel panel = new JPanel();
        panel.setLayout(new BoxLayout(panel, BoxLayout.Y_AXIS));
        panel.setBackground(new Color(245, 245, 220)); // Set background color to beige

        JLabel guideLabel = new JLabel("Guide");
        guideLabel.setFont(new Font("Arial", Font.BOLD, 24)); // Increased font size
        guideLabel.setAlignmentX(Component.CENTER_ALIGNMENT);
        guideLabel.setForeground(new Color(139, 69, 19)); // Set text color to brown
        panel.add(guideLabel);

        panel.add(Box.createRigidArea(new Dimension(0, 5))); // Reduced space between Guide and GuideText

        JTextArea guideText = new JTextArea(
                "1. Click on the Sign in/Sign up tab.\n" +
                        "2. Click on Register if you're not registered yet; otherwise, click on Login.\n" +
                        "3. After login, you will be redirected to the adoption form.\n" +
                        "4. Fill out the adoption form to adopt a pet.\n" +
                        "5. Click on the Adoption tab to check the availability of pets."
        );
        guideText.setEditable(false);
        guideText.setLineWrap(true);
        guideText.setWrapStyleWord(true);
        guideText.setFont(new Font("Arial", Font.PLAIN, 18)); // Increased font size
        guideText.setBackground(new Color(245, 245, 220)); // Set background color to beige
        guideText.setAlignmentX(Component.CENTER_ALIGNMENT); // Center the text area
        panel.add(guideText);

        panel.add(Box.createRigidArea(new Dimension(0, 20))); // Reduced space between GuideText and ContactLabel

        JLabel contactLabel = new JLabel("Contact for More Details");
        contactLabel.setFont(new Font("Arial", Font.BOLD, 24)); // Increased font size
        contactLabel.setAlignmentX(Component.CENTER_ALIGNMENT);
        contactLabel.setForeground(new Color(139, 69, 19)); // Set text color to brown
        panel.add(contactLabel);

        panel.add(Box.createRigidArea(new Dimension(0, 5))); // Reduced space between ContactLabel and ContactText

        JTextArea contactText = new JTextArea(
                "Shreya Sinalkar: 9463728822\n" +
                        "Arya Shinde: 9489236270"
        );
        contactText.setEditable(false);
        contactText.setLineWrap(true);
        contactText.setWrapStyleWord(true);
        contactText.setFont(new Font("Arial", Font.PLAIN, 18)); // Increased font size
        contactText.setBackground(new Color(245, 245, 220)); // Set background color to beige
        contactText.setAlignmentX(Component.CENTER_ALIGNMENT); // Center the text area
        panel.add(contactText);

        add(panel, BorderLayout.CENTER);

        setVisible(true);
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(Help::new);
    }
}
package PetAdopt;

import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class ALogin extends JFrame {

    ALogin() {
        setSize(400, 300);
        setLocation(600, 300);
        setLayout(null);

        // Adding JLabel for "Login" text
        JLabel loginLabel = new JLabel("Login");
        loginLabel.setFont(new Font("Arial", Font.BOLD, 20));
        loginLabel.setForeground(Color.BLUE);
        loginLabel.setBounds(150, 10, 100, 30);
        add(loginLabel);

        JLabel l1 = new JLabel("Username: ");
        l1.setBounds(40, 50, 100, 30);
        add(l1);

        JTextField t1 = new JTextField();
        t1.setBounds(150, 50, 150, 30);
        add(t1);

        JLabel l2 = new JLabel("Password: ");
        l2.setBounds(40, 100, 100, 30);
        add(l2);

        JPasswordField t2 = new JPasswordField();
        t2.setBounds(150, 100, 150, 30);
        add(t2);

        JButton b1 = new JButton("Login");
        b1.setBackground(Color.BLACK);
        b1.setForeground(Color.WHITE);
        b1.setBounds(120, 160, 100, 30);
        add(b1);
        b1.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String username = t1.getText();
                String password = String.valueOf(t2.getPassword());

                // Validate fields
                if (username.isEmpty() || password.isEmpty()) {
                    JOptionPane.showMessageDialog(null, "Please fill in all fields.");
                } else {
                    if (validateUser(username, password)) {
                        dispose(); // Close the login window
                        new Adopt(); // Open the adoption form
                    } else {
                        JOptionPane.showMessageDialog(null, "User doesn't exist or incorrect password!");
                    }
                }
            }
        });

        JButton b2 = new JButton("Cancel");
        b2.setBackground(Color.BLACK);
        b2.setForeground(Color.WHITE);
        b2.setBounds(240, 160, 100, 30);
        add(b2);
        b2.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                dispose(); // Close the login window
            }
        });

        setVisible(true);
    }

    // Method to validate user credentials
    private boolean validateUser(String username, String password) {
        Connection con = null;
        PreparedStatement pst = null;
        ResultSet rs = null;
        boolean isValid = false;
        try {
            // Establish connection
            con = DriverManager.getConnection("jdbc:oracle:thin:@//LAPTOP-K81RSSBS:1521/orcl", "system", "arya2901");
            // Check if the username and password match
            pst = con.prepareStatement("SELECT * FROM users WHERE username = ? AND password = ?");
            pst.setString(1, username);
            pst.setString(2, password);
            rs = pst.executeQuery();
            if (rs.next()) {
                isValid = true;
            }
        } catch (SQLException ex) {
            ex.printStackTrace();
        } finally {
            try {
                // Close resources
                if (rs != null) rs.close();
                if (pst != null) pst.close();
                if (con != null) con.close();
            } catch (SQLException ex) {
                ex.printStackTrace();
            }
        }
        return isValid;
    }
}
package PetAdopt;

import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class Reg extends JFrame {

    Reg() {
        setSize(400, 300);
        setLocation(600, 300);
        setLayout(null);

        // Adding JLabel for "Register" text
        JLabel registerLabel = new JLabel("Register");
        registerLabel.setFont(new Font("Arial", Font.BOLD, 20));
        registerLabel.setForeground(Color.BLUE);
        registerLabel.setBounds(140, 10, 100, 30);
        add(registerLabel);

        JLabel l1 = new JLabel("Username: ");
        l1.setBounds(40, 50, 100, 30);
        add(l1);

        JTextField t1 = new JTextField();
        t1.setBounds(150, 50, 150, 30);
        add(t1);

        JLabel l2 = new JLabel("Password: ");
        l2.setBounds(40, 100, 100, 30);
        add(l2);

        JPasswordField t2 = new JPasswordField();
        t2.setBounds(150, 100, 150, 30);
        add(t2);

        JButton b1 = new JButton("Register");
        b1.setBackground(Color.BLACK);
        b1.setForeground(Color.WHITE);
        b1.setBounds(120, 160, 100, 30);
        add(b1);
        b1.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String username = t1.getText();
                String password = String.valueOf(t2.getPassword());

                // Validate fields
                if (username.isEmpty() || password.isEmpty()) {
                    JOptionPane.showMessageDialog(null, "Please fill in all fields.");
                } else {
                    if (insertUser(username, password)) {
                        dispose(); // Close the register window
                    }
                }
            }
        });

        JButton b2 = new JButton("Cancel");
        b2.setBackground(Color.BLACK);
        b2.setForeground(Color.WHITE);
        b2.setBounds(240, 160, 100, 30);
        add(b2);
        b2.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                dispose(); // Close the register window
            }
        });

        setVisible(true);
    }

    // Method to insert user into the database
    private boolean insertUser(String username, String password) {
        Connection con = null;
        PreparedStatement pst = null;
        ResultSet rs = null;
        boolean isInserted = false;
        try {
            // Establish connection
            con = DriverManager.getConnection("jdbc:oracle:thin:@//LAPTOP-K81RSSBS:1521/orcl", "system", "arya2901");
            // Check if the username already exists
            pst = con.prepareStatement("SELECT * FROM users WHERE username = ?");
            pst.setString(1, username);
            rs = pst.executeQuery();
            if (rs.next()) {
                // Username already exists, show popup and return false
                JOptionPane.showMessageDialog(null, "Username already exists!");
            } else {
                // Username does not exist, proceed with registration
                pst = con.prepareStatement("INSERT INTO users (username, password) VALUES (?, ?)");
                pst.setString(1, username);
                pst.setString(2, password);
                int rowsInserted = pst.executeUpdate();
                if (rowsInserted > 0) {
                    isInserted = true;
                    JOptionPane.showMessageDialog(null, "User registered successfully!");
                }
            }
        } catch (SQLException ex) {
            ex.printStackTrace();
        } finally {
            try {
                // Close resources
                if (rs != null) rs.close();
                if (pst != null) pst.close();
                if (con != null) con.close();
            } catch (SQLException ex) {
                ex.printStackTrace();
            }
        }
        return isInserted;
    }
}[Uploading PetAdooptionPortal…]()
