## [Excel Mayhem](https://pearlctf.in/challenges#Excel%20Mayhem-36)

This write-up details how to solve the Excel Mayhem challenge from pearlCTF.

**Initial Difficulty:**

The challenge presented an Excel file protected with a password, preventing us from easily viewing the content. 

**Solution Steps:**

1. **Opening the File (Linux Method):**
   - I bypassed the password protection by using Linux. 
   **Note:** While binwalk can be used for file extraction, in this case, simply changing the file extension from `.xlsx` to `.zip` would have achieved the same result. 
   - Then I extracted the file contents and navigated to the folder containing the worksheet data (sheet1.xml).

2. **Removing File Protection:**
   - Searched the `sheet1.xml` file for the keyword "protect".
   - I found a line mentioning 
   ```xml
   <sheetProtection algorithmName="SHA-512" ... />
   ```
   - Everything from this line to the closing `</worksheet>` tag was removed. This effectively removed the password protection from the Excel file.

3. **Extracting the Flag:**
   - The modified `.xml` file was saved, compressed back into a `.zip` archive, and the extension was changed back to `.xlsx`.
   - Opening the unprotected file allowed us to auto-fit the columns, revealing the data.
   - By zooming out, we noticed a pattern change in the data containing the flag `h3ll_0f_4n_3xc3l` located in cell `AE:34`.

**The Flag:**

The final flag for this challenge is:

```
pearl{h3ll_0f_4n_3xc3l}
```
