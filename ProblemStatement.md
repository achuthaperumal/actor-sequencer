# Problem Statemenet
A customer has asked you to design a complete product verification solution.  This program will be used on the manufacturing floor to certify that a product is ready for shipment.  The main program should allow the operator to select from a list of available tests.  Once the tests are selected, the operator will click a button to run the tests.  The progress of the tests should be displayed on the front panel.  When complete, a PASS/FAIL result for each test should be displayed and a report (preferably HTML) should be created.

## User Interface 

The user interface (UI) should provide the following items: 

[ ] List of available tests (with selection capabilities) 

[ ] Ability to start the selected group of tests 

[ ] A status message display (test will send status messages to the UI). 

[ ] Overall PASS/FAIL indicator 

[ ] Test-by-test PASS/FAIL indicator 

[ ] Print Results button 

[ ] Exit Button 

Anything extra you can think to improve the interface.

The list of tests should not be hard coded but instead read from a file.  For an added bonus, you can demonstrate the ability to call DLL’s from LabVIEW.  One idea would be to call the ‘GetComputerNameA’ function from the kernel32.dll to add the name of the computer that performed the test to the report.  A different DLL call can be made in place of this one.

## Tests 

The tests should not be subVIs of the UI.  Instead, the tests should be called dynamically to promote modularity.  There will be three simple tests that attempt to mimic situations that are common.  Each is discussed below.  The tests are so short that they should run too fast to be able to see any of the status messages on the main UI.  If that is the case, add some delay to the tests to make them more closely match real world timing.  The tests should utilize a common method of sending status updates to the UI. 

### Test 1: Network Connectivity 

This test verifies that multiple network interfaces on a device are functioning properly.  The test will use the ping utility (command line, Window’s API, or other method) to insure network traffic.  Two interfaces will be pinged 20 times each.  The test will pass if 80% or more of the packets return.  The first address will be to the loop back address (127.0.0.1) and should pass.  The second address should be to some random address that should fail (i.e. 10.1.2.3).  Ping the address and report back the percent returned. 

### Test 2: Text Based Communications 

This test verifies that an interface is properly communicating and internal self-tests provide passing results.  The attached file (test2.txt) supplies a collection of 5 responses from the device.  Each response must be parsed one at a time.  

The responses are formatted:
``` 
Response1{Actual response here} 

Response2{Actual response here} 

… 

Response5{Actual response here} 
```
 

For each response there are 4 results. Verify that: 
```
Result1 returns ‘PASSED’ 

Result2 is greater than or equal to 3.30 volts 

Result3 is greater than or equal to 80% 

Result4 is exactly 6 characters long and contains only valid HEX numbers 
```
 

A lot of what is required for device testing is parsing text based results.  This test is meant to see how well you can parse the provided commands. 

### Test 3: Protocol Verification 

A lot of devices use CAN bus to communicate with test stations.  This test verifies the ability to parse the returned data.  The attached file (test3.bin) is a binary file containing a group of TLV’s.  TLV stands for Type/Length/Value.  In this file the type and length are single bytes and the value is LENGTH bytes long.  Multiple TLV’s are strung together into a single stream of bytes.  For example: 

 

`T1L1V1T2L2V2T3L3V3 … TnLnVn` 

`(hex code) 05 04 FF FF FF FF 01 01 FF 06 07 FF FF FF FF FF FF FF `

 

For this test, open the binary file and walk through the TLVs.  You’re looking for `Type = 5`.  For each `type 5`, the value is a string that represents a floating-point number.  Read the correct number of bytes, convert that to string, and then convert it to a number.  Pass that TLV if the number is greater that 3.30.  There should be a separate PASS/FAIL for each occurrence of the Type 5 TLV.
