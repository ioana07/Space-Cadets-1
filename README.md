
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.URL;
 
public class GetInfo {
   
    public static void main(String args[]) throws Exception {
       
        String user = read_ID(); 
        String[] new_data = new String[1]; 
        new_data = getData(user); 
        displayArray(new_data, 2); 
       
    }
   
   
    public static String read_ID() throws Exception {
       
        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
        String ID;
       
        System.out.println("Enter user ID: ");
        ID = reader.readLine();
        reader.close();
       
        return ID;
       
    }
   
  
    public static String[] getData(String user) throws Exception {
   
        String[] person_data = new String[2];
       
        URL url = new URL("http://www.ecs.soton.ac.uk/people/" + user); 
        BufferedReader page_reader = new BufferedReader(new InputStreamReader(url.openStream())); 
       
        String input_line; 
        while ((input_line = page_reader.readLine()) != null) { 
            if (input_line.contains("<title>")) { 
               
                int end_of_name = input_line.indexOf("|"); 
                String name; 
                name = input_line.substring(11,end_of_name - 1); 
               
               
                if (name.equals("People")) {
                    name = "-No Such Person Found-";
                    person_data[1] = "Job Title: N/A";
                }
               
                person_data[0] = "Name: " + name; 
               
            } else if (input_line.contains("property=\"jobTitle\"")) { 
               
                int start_of_prop = input_line.indexOf("property=\"jobTitle\">");
                char current = ' '; 
                int counter = start_of_prop + 20; 
                while ( current != '<') {
                    current = input_line.charAt(counter);
                    counter++;
                }
                String job_title;
                job_title = input_line.substring(start_of_prop + 20,counter-1);  
                person_data[1] = "Job Title: " + job_title; 
               
            }
        }
        page_reader.close();
       
        return person_data;
       
    }
 

    public static void displayArray(String[] array, int length) {
   
        for (int i = 0 ; i < length ; i++ ) {
           
            System.out.println(array[i]);
           
        }
       
    }
   
}
