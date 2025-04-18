# Context aware testing:


    # Function to generate test cases using GPT
    def generate_test_cases(context, num_cases=5):
        prompt = f"""
        You are an expert in financial transactions and risk assessment. Generate {num_cases} test cases for the following scenario:
    
        Scenario: {context}
        
        Each test case should include:
        - Test Case ID
        - Description
        - Expected Outcome
        - Risk Level (Low, Medium, High)
        - Regulatory Compliance Notes (if applicable)
        
        Format the output as a structured JSON list.
        """
    
        response = openai.ChatCompletion.create(
            model="gpt-4",
            messages=[{"role": "system", "content": "You are an AI test case generator."},
                      {"role": "user", "content": prompt}]
        )
    
        return response["choices"][0]["message"]["content"]

    # Example usage
    context = "Detect fraudulent credit card transactions based on unusual spending patterns."
    test_cases = generate_test_cases(context)
    print(test_cases)



# Ananoly detection and reconciliation


    # Load historical and current data
    historical_data = pd.read_csv("historical_data.csv")  # Replace with actual data file
    current_data = pd.read_csv("current_data.csv")  # Replace with actual data file
    
    # Selecting numerical features for analysis
    features = historical_data.select_dtypes(include=[np.number])
    
    # Train IsolationForest on historical data
    model = IsolationForest(contamination=0.05, random_state=42)  # Adjust contamination rate
    model.fit(features)
    
    # Predict anomalies in current data
    current_features = current_data.select_dtypes(include=[np.number])
    current_data["Anomaly"] = model.predict(current_features)
    
    # -1 indicates anomaly, 1 indicates normal data
    anomalies = current_data[current_data["Anomaly"] == -1]
    
    # Function to generate insights using GPT-4
    def generate_gpt_insight(data_row):
        prompt = f"""
        The following data point has been detected as an anomaly:

        {data_row.to_dict()}
    
        Please provide a possible reason why this data point might be an anomaly and suggest potential reconciliation steps.
        """
    
        response = openai.ChatCompletion.create(
            model="gpt-4",
            messages=[{"role": "user", "content": prompt}]
        )
    
        return response["choices"][0]["message"]["content"]
    
    # Generate insights for detected anomalies
    if not anomalies.empty:
        anomalies["GPT_Insight"] = anomalies.apply(generate_gpt_insight, axis=1)
    
    # Save results
    current_data.to_csv("anomaly_results_with_gpt.csv", index=False)
    
    print("Anomaly detection completed. Check 'anomaly_results_with_gpt.csv' for results.")


#Print

import java.awt.print.PrinterJob;
import java.io.File;
import java.io.IOException;
import javax.print.PrintService;
import javax.print.PrintServiceLookup;

import org.apache.pdfbox.pdmodel.PDDocument;
import org.apache.pdfbox.printing.PDFPageable;

public class PDFPrinter {

    public static void main(String[] args) {
        String pdfPath = "C:/Users/smitb/OneDrive/Desktop/Poster.pdf";

        try (PDDocument document = PDDocument.load(new File(pdfPath))) {
            PrinterJob job = PrinterJob.getPrinterJob();
            
            // Automatically select default printer
            PrintService defaultPrintService = PrintServiceLookup.lookupDefaultPrintService();
            if (defaultPrintService != null) {
                job.setPrintService(defaultPrintService);
            }

            job.setPageable(new PDFPageable(document));
            
            // Print without showing a print dialog
            job.print();

            System.out.println("Printing completed.");
        } catch (IOException e) {
            System.err.println("Error loading PDF: " + e.getMessage());
        } catch (Exception e) {
            System.err.println("Error printing PDF: " + e.getMessage());
        }
    }
}





