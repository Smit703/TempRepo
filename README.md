import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.util.List;
import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;

public class GitHubFolderDownloader {

    // Replace with your repository and folder
    private static final String REPO_OWNER = "your-username";
    private static final String REPO_NAME = "your-repository";
    private static final String FOLDER_PATH = "path-to-folder";
    private static final String GITHUB_API_URL = "https://api.github.com/repos/" + REPO_OWNER + "/" + REPO_NAME + "/contents/" + FOLDER_PATH;

    public static void main(String[] args) throws Exception {
        HttpClient client = HttpClient.newHttpClient();
        
        // Send GET request to fetch the list of files
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(GITHUB_API_URL))
                .header("Accept", "application/vnd.github.v3+json")
                .build();

        HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());

        // Parse the JSON response
        ObjectMapper objectMapper = new ObjectMapper();
        JsonNode filesArray = objectMapper.readTree(response.body());

        if (filesArray.isArray()) {
            for (JsonNode fileNode : filesArray) {
                String fileName = fileNode.get("name").asText();
                String fileUrl = fileNode.get("download_url").asText();

                // Download each file content
                String fileContent = downloadFileContent(client, fileUrl);
                System.out.println("File: " + fileName);
                System.out.println(fileContent);
            }
        } else {
            System.out.println("No files found in the folder.");
        }
    }

    // Helper method to download the file content from GitHub
    private static String downloadFileContent(HttpClient client, String fileUrl) throws Exception {
        HttpRequest fileRequest = HttpRequest.newBuilder()
                .uri(URI.create(fileUrl))
                .build();

        HttpResponse<String> fileResponse = client.send(fileRequest, HttpResponse.BodyHandlers.ofString());
        return fileResponse.body();
    }
}
