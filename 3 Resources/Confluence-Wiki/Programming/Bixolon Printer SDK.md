---
title: "Bixolon Printer SDK"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/46989870505/Bixolon+Printer+SDK
space: "Helios"
topic: programming
relevance: 0.842
depth: 3
updated: 2021-10-25
attachments: 0
tags:
  - confluence
  - programming
  - space/helios
---

# Bixolon Printer SDK

> [!info] Imported from Confluence
> Space **Helios** · updated 2021-10-25 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/46989870505/Bixolon+Printer+SDK)
> Relevance 0.842 · topic `programming`

**1. SDK Download (Android SDK):**  
<a href="https://bixolon.com/download_view.php?idx=42#" class="external-link" data-card-appearance="inline" rel="nofollow">https://bixolon.com/download_view.php?idx=42#</a>

**2. Setup SDK:**  
Import below files in SDK to project:

<div>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<th><p>Library location/ Name</p></th>
<th><p>Description</p></th>
</tr>
&#10;<tr>
<td><p>libs/bixolon_printer_Vxxx.jar</p></td>
<td><p>Implementation of JavaPOS service<br />
component layers / printer setting library</p></td>
</tr>
<tr>
<td><p>libs/libcommon_Vxxx.jar</p></td>
<td><p>Printer control core library</p></td>
</tr>
<tr>
<td><p>libs/jniLibs/ABI type/libbxl_common.so</p></td>
<td><p>Printer control native library</p></td>
</tr>
</tbody>
</table>

</div>

**3. Connect Printer:**

- Write device setting information to be connected

  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="fdeee1e7-7711-4a88-9f86-affa5e3e9af1" macro-name="code" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  // BXLConfigLoader creating / setting file open
  BXLConfigLoader bxlConfigLoader = new BXLConfigLoader(this);
  bxlConfigLoader.openFile();

  // Adding device information
  bxlConfigLoader.addEntry("SRP-Q302",              // Logical Name (nickname)
      BXLConfigLoader.DEVICE_CATEGORY_POS_PRINTER,  // Device Category
      BXLConfigLoader.PRODUCT_NAME_SRP_Q302,        // Product Name
      BXLConfigLoader.DEVICE_BUS_BLUETOOTH,         // Interface Type
      "74:F0:7D:E4:11:AF");                         // MAC or IP address of the device
      
  // Saving setting file
  bxlConfigLoader.saveFile();
  ```

  </div>

  </div>

- Open printer & enable device

  <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="95cf3175-601a-4204-b09d-408ff6e82dc2" macro-name="code" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  POSPrinter posPrinter = new POSPrinter(this);
  int timeout = 5000;

  posPrinter.open("SRP-Q302");       // "SRP-Q302" is logical name
  posPrinter.claim(timeout);         // Attempt to open the port for the time specified
  posPrinter.setDeviceEnabled(true); // Enable device for use
  ```

  </div>

  </div>

**4. Print Receipt:**

- Format Print Page:

  - Print area setting:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cd03f862-e914-4996-accf-ce2b19283cf9" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    // X coordinate of area, Y coordinate of area, width of area, height of area
    posPrinter.setPageModePrintArea("0,0,384,1200");
    ```

    </div>

    </div>

  - Print direction setting:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="79178f4a-fcfa-4d4f-a487-fd83e659f62d" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    // Options: PTR_PD_LEFT_TO_RIGHT, PTR_PD_BOTTOM_TO_TOP, PTR_PD_RIGHT_TO_LEFT, PTR_PD_TOP_TO_BOTTOM
    posPrinter.setPageModePrintDirection(POSPrinterConst.PTR_PD_LEFT_TO_RIGHT); 
    ```

    </div>

    </div>

  - Mode change:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8e80578b-56fb-47f8-af8e-cb67c3849a10" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    /* Options: 
      + PTR_PM_PAGE_MODE: Enable page mode, 
      + PTR_PM_NORMAL: Change to the normal mode and the data stored in the page mode buffer is printed*/
    posPrinter.pageModePrint(POSPrinterConst.PTR_PM_PAGE_MODE);
    ```

    </div>

    </div>

  - Specifies the print start position:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="cc70d3d3-f296-417e-a1f6-1b0dc5d4db38" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    posPrinter.setPageModeHorizontalPosition(130);
    posPrinter.setPageModeVerticalPosition(200);
    ```

    </div>

    </div>

- Print Receipt:

  - Prints barcodes:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="94d24dff-23b1-4082-b3ac-5da474335972" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    posPrinter.printBarCode(
          POSPrinterConst.PTR_S_RECEIPT,      // Fixed value
          "123456789",                        // The data to be included in the barcode
          POSPrinterConst.PTR_BCS_QRCODE,     // Select the type of barcode
          8,                                  // Specify the height of the barcode
          8,                                  // Specify the width of the barcode
          POSPrinterConst.PTR_BC_CENTER,      // Select the alignment of the barcode
          POSPrinterConst.PTR_BC_TEXT_BELOW); // Determine the postion of the text to be printed with the barcode
    ```

    </div>

    </div>

  - Prints Image (file):

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a6adca63-6457-4bc7-94ae-e74d5169f001" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    // Image printing options
    ByteBuffer buffer = ByteBuffer.allocate(4);
    buffer.put((byte) POSPrinterConst.PTR_S_RECEIPT); // Fixed value
    buffer.put((byte) 80);    // Brightness (0 ~ 100)
    buffer.put((byte) 0x01);  // Compression algorithm (0x00: None, 0x01: RLE, 0x02 : LZMA)
    buffer.put((byte) 0x00);  // Reserved byte

    //Prints image. (file printing)
    posPrinter.printBitmap(
          buffer.getInt(0),               // Set image printing options 
          imagePath,                      // Specify the path to the image file
          posPrinter.getRecLineWidth(),   // Specify the image width
          POSPrinterConst.PTR_BM_LEFT);   // Select the image alignment
    ```

    </div>

    </div>

  - Prints image (bitmap):

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="127adbe6-adca-41a2-98ea-277ac0cec490" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    // Image printing options
    ByteBuffer buffer = ByteBuffer.allocate(4);
    buffer.put((byte) POSPrinterConst.PTR_S_RECEIPT); // Fixed value
    buffer.put((byte) 80);    // Brightness (0 ~ 100)
    buffer.put((byte) 0x01);  // Compression algorithm (0x00: None, 0x01: RLE, 0x02 : LZMA)
    buffer.put((byte) 0x00);  // Reserved byte

    posPrinter.printBitmap(
          buffer.getInt(0),               // Set image printing options 
          BitmapData,                     // Specify the bitmap data
          posPrinter.getRecLineWidth(),   // Specify the image width
          POSPrinterConst.PTR_BM_LEFT);   // Select the image alignment
    ```

    </div>

    </div>

  - Print normal:

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a52d6e96-b2b2-4a98-9ec0-41673fa6e7d5" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    posPrinter.printNormal(
          POSPrinterConst.PTR_S_RECEIPT, // Fixed value
          "Print Data\n");               // Specify the data to be printed
    ```

    </div>

    </div>
