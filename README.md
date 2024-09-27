# TempRepo

public repo

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;

public class GitHubFileReader {
    public static void main(String[] args) {
        String fileURL = "https://raw.githubusercontent.com/user/repo/main/file.txt";  // Replace with your file's raw URL
        try {
            // Create a URL object
            URL url = new URL(fileURL);
            
            // Open a connection to the URL
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            
            // Set the request method to GET
            connection.setRequestMethod("GET");
            
            // Get the response code
            int responseCode = connection.getResponseCode();
            
            if (responseCode == HttpURLConnection.HTTP_OK) {
                // Create a BufferedReader to read the response
                BufferedReader in = new BufferedReader(new InputStreamReader(connection.getInputStream()));
                String inputLine;
                StringBuilder content = new StringBuilder();
                
                // Read the response line by line
                while ((inputLine = in.readLine()) != null) {
                    content.append(inputLine).append("\n");
                }
                
                // Close the connections
                in.close();
                connection.disconnect();
                
                // Print or process the content of the file
                System.out.println("File content:\n" + content.toString());
            } else {
                System.out.println("Failed to retrieve the file. Response code: " + responseCode);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}


private repo
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.Base64;

public class GitHubPrivateFileReader {
    public static void main(String[] args) {
        String apiUrl = "https://api.github.com/repos/username/repo/contents/path/to/file.txt";  // Replace with API URL
        String personalAccessToken = "your_personal_access_token";  // Replace with your GitHub token
        
        try {
            // Create a URL object
            URL url = new URL(apiUrl);
            
            // Open a connection to the URL
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            
            // Set the request method to GET
            connection.setRequestMethod("GET");
            
            // Add Authorization header for GitHub API
            String encodedAuth = "token " + personalAccessToken;
            connection.setRequestProperty("Authorization", encodedAuth);
            
            // Get the response code
            int responseCode = connection.getResponseCode();
            
            if (responseCode == HttpURLConnection.HTTP_OK) {
                // Create a BufferedReader to read the response
                BufferedReader in = new BufferedReader(new InputStreamReader(connection.getInputStream()));
                String inputLine;
                StringBuilder content = new StringBuilder();
                
                // Read the response line by line
                while ((inputLine = in.readLine()) != null) {
                    content.append(inputLine).append("\n");
                }
                
                // Close the connections
                in.close();
                connection.disconnect();
                
                // Print or process the content of the file
                System.out.println("File content:\n" + content.toString());
            } else {
                System.out.println("Failed to retrieve the file. Response code: " + responseCode);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}


import java.util.Base64;

String encodedContent = content.toString();  // Replace with actual encoded response content
byte[] decodedBytes = Base64.getDecoder().decode(encodedContent);
String decodedContent = new String(decodedBytes);
System.out.println("Decoded File Content:\n" + decodedContent);

