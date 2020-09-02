# Repositoryjava

package batch_layoutmycration;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;


public class batchLayout {
	
	private final String className = "com.mysql.jdbc.Driver";
	private final String url = "jdbc:mysql://localhost:3306/students";
    private final String user ="root";
    private final String pass ="";
 
    private Connection connection;
    
    private void connect() {
        try {
            Class.forName(className);
            connection = DriverManager.getConnection(url, user, pass);
            System.out.println("Connect success!");
        } catch (ClassNotFoundException e) {
            System.out.println("Class not found!");
        } catch (SQLException e) {
            System.out.println("Error connection!");
        }
    }
    private ResultSet getDataId(int id) {
		ResultSet rs = null;
		//String sql = "select * from infolayout where id =" + id; cach nay co 1 so han che
		//Statement st ;
		String sql = "select * from infolayout where `id` = ?" ;
		PreparedStatement pst = null;
		try {
			pst = connection.prepareStatement(sql);
			pst.setInt(1, id);
			rs = pst.executeQuery();
        } catch (SQLException e) {
            System.out.println("select error \n" + e.toString());
        }
		
		return rs;
    }
	private ResultSet getData() {
		ResultSet rs = null;
		String sql = "select * from infolayout";
		Statement st ;
		try {
			st = connection.createStatement();
			rs = st.executeQuery(sql);
        } catch (SQLException e) {
            System.out.println("select error \n" + e.toString());
        }
		
		return rs;
	}
	
	private void showData (ResultSet rs) {
		try {
			while (rs.next()) {
				System.out.printf("%-10s %-20s %-5.2s \n", rs.getInt(1), rs.getString(2), rs.getString(3));
			}
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	
	// delete by Id
    private void deleteId(int id) {
        String sqlCommand = "delete from infolayout where id = ?";
        PreparedStatement pst = null;
        try {
            pst = connection.prepareStatement(sqlCommand);
            pst.setInt(1, id);
            if (pst.executeUpdate() > 0) {
                System.out.println("delete success");
            } else {
                System.out.println("delete error \n");
            }
        } catch (SQLException e) {
            System.out.println("delete error \n" + e.toString());
        }
    }
    
    // insert student s to database
    private void insert(student s) {
        String sqlCommand = "insert into infolayout value(?, ?, ?, ?)";
        PreparedStatement pst = null;
        try {
            pst = connection.prepareStatement(sqlCommand);
            // replace three "?" by id, Name and point of Studnet s
            pst.setInt(1, s.getId());
            pst.setString(2, s.getUserName());
            pst.setString(3, s.getWid_def());
            pst.setString(4, s.getDescretion());
            if (pst.executeUpdate() > 0) {
                System.out.println("insert success");
            } else {
                System.out.println("insert error \n");
            }
        } catch (SQLException e) {
            System.out.println("insert error \n" + e.toString());
        }
    }
    
    // update studetn by Id
    private void updateId(int id, student s) {
        String sqlCommand = "update infolayout set userName = ?, wid_def = ?, descretion = ? where id = ?";
        PreparedStatement pst = null;
        try {
            pst = connection.prepareStatement(sqlCommand);
            pst.setString(1, s.getUserName());
            pst.setString(2, s.getWid_def());
            pst.setString(3, s.getDescretion());
            pst.setInt(4, s.getId());
            if (pst.executeUpdate() > 0) {
                System.out.println("update success");
            } else {
                System.out.println("update error \n");
            }
        } catch (SQLException e) {
            System.out.println("update error \n" + e.toString());
        }
    }
	/**
	 * @param args
	 */
	public static void main(String[] args) {
		batchLayout myConnect = new batchLayout();
        myConnect.connect();
        //myConnect.showData(myConnect.getData());
        // myConnect.deleteId(1);
        //myConnect.insert(new student(4,"minh","123442", "new"));
        myConnect.updateId(4, new student(4,"minh","123442", "new 123"));
        myConnect.showData(myConnect.getData());
	}
	/*
	 * 1          HAU                  12    
	   2          HIEN                 23    
	   3          CHUONG               12  
	 * 
	 */
}
