#IT 

**Method 1:** Using **msinfo32 (System Information Tool)**
This is the most comprehensive built-in tool for generating a detailed system report.

Open System Information:

Press "Window" key + R, type msinfo32, and press Enter.
or: search for "System Information" in the Start menu.

Create the report:

In the System Information window, go to File > Export.
Choose a location and filename (e.g., System_Report.txt).

Click Save. The report will be saved a .txt or .nfo file.

What’s Included?

Hardware: Processor, RAM, motherboard, storage, graphics, etc.
Software: OS version, installed updates, drivers, and running services.

### **Method 3: Using PowerShell (Detailed Hardware/Software)**

For a customizable report:

1. Open **PowerShell** as Administrator.
    
2. Run:
    powershell
    
    Copy
    
    ```
    Get-ComputerInfo | Out-File -FilePath "C:\System_Report.txt"
    ```
    
    - This exports a detailed list of hardware, OS, and software info.
    - 
3. **For Hardware Only:**
    
    powershell
    
    Copy
    
    ```
    Get-WmiObject -Class Win32_ComputerSystem | Format-List * > C:\Hardware_Report.txt
    ```
    

---
