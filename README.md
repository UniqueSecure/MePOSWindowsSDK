# MePOS Connect windows SDK Guide V1.1

The MePOS connect SDK is designed to allow communication from a tablet or Windows computer to the MePOS host
unit. SDK libraries are currently available for Android and Windows.

This document is a reference on how to integrate to the MePOS unit into your own tablet application. This document
does not include information on how to set up the MePOS unit, for this please refer to the documentation that came with
your MePOS unit.

## Contents

- [Supported tablet devices](#supported-tablet-devices)
- [Supported MePOS firmware](#supported-mepos-firmware)
- [Use of the MePOS connect SDK on Windows](#use-of-the-mepos-connect-sdk-on-windows)
- [Libraries](#libraries)
- [References](#references)
- [Creating a new MePOS object](#creating-a-new-mepos-object)
- [MePOS SDK Methods](#mepos-sdk-methods)
    - [void enableWifi()](#void-enablewifi)
    - [void disableWifi()](#void-disablewifi)
    - [void enableUSB()](#void-enableusb)
    - [void disableUSB()](#void-disableusb)
    - [int cashDrawerStatus()](#int-cashdrawerstatus)
    - [bool openCashDrawer()](#bool-opencashdrawer)
    - [bool openCashDrawer(bool validateCashDrawerStatus)](#bool-opencashdrawerbool-validatecashdrawerstatus)
    - [int setDiagnosticLed(int position, int colour)](#int-setdiagnosticledint-position-int-colour)
    - [int setCosmeticLedCol(int colour)](#int-setcosmeticledcolint-colour)
    - [int print(MePOSReceipt receipt, MePOSPrinterCallback callback)](#int-printmeposreceipt-receipt-meposprintercallback-callback)
    - [int print(MePOSReceipt receipt)](#int-printmeposreceipt-receipt)
    - [int printRAW(String rawData)](#int-printrawstring-rawdata)
    - [int serialRAW(String rawData)](#int-serialrawstring-rawdata)
    - [bool printerBusy()](#bool-printerbusy)
    - [void printConfigPage()](#void-printconfigpage)
    - [String getFWVersion()](#string-getfwversion)
    - [String getSerialNumber()](#string-getserialnumber)
    - [MePOSConnectionManager getConnectionManager()](#meposconnectionmanager-getconnectionmanager)
- [MePOSConnectionManager](#meposconnectionmanager)
    - [int getConnectionStatus()](#int-getconnectionstatus)
    - [void setConnectionIPAddress(string IPAddress)](#void-setconnectionipaddressstring-ipaddress)
    - [String getConnectionIPAddress()](#string-getconnectionipaddress)
    - [String MePOSGetAssignedIP()](#string-meposgetassignedip)
    - [bool MePOSConnectDefault()](#bool-meposconnectdefault)
    - [bool MePOSConnectDefault(MePOSConfigurationCallback callback)](#bool-meposconnectdefaultmeposconfigurationcallback-callback)
    - [bool MePOSConnectEthernet(String IPAddress, String netmask)](#bool-meposconnectethernetstring-ipaddress-string-netmask)
    - [bool MePOSConnectEthernet(String ipAddress, String netmask, MePOSConfigurationCallback callback)](#bool-meposconnectethernetstring-ipaddress-string-netmask-meposconfigurationcallback-callback)
    - [bool MePOSConnectEthernet(MePOSConfigurationCallback callback)](#bool-meposconnectethernetmeposconfigurationcallback-callback)
    - [bool MePOSConnectEthernet()](#bool-meposconnectethernet)
    - [bool MePOSConnectWiFi(String SSID, String IPAddress, String netmask, String encryption, String password)](#bool-meposconnectwifistring-ssid-string-ipaddress-string-netmask-string-encryption-string-password)
    - [bool MePOSConnectWiFi(String ssid, String ipAddress, String netmask, String encryption, String password, MePOSConfigurationCallback callback)](#bool-meposconnectwifistring-ssid-string-ipaddress-string-netmask-string-encryption-string-password-meposconfigurationcallback-callback)
    - [bool MePOSSetAccessPoint(String SSID, String encryption, String password)](#bool-mepossetaccesspointstring-ssid-string-encryption-string-password)
    - [bool MePOSSetAccessPoint(String ssid, String encryption, String password, MePOSConfigurationCallback callback)](#bool-mepossetaccesspointstring-ssid-string-encryption-string-password-meposconfigurationcallback-callback)
    - [String getMACAddress()](#string-getmacaddress)
    - [String getSSID()](#string-getssid)
    - [String getRouterFirmware()](#string-getrouterfirmware)
- [MePOSReceipt](#meposreceipt)
    - [setCutType(int cutType)](#setcuttypeint-cuttype)
    - [MePOSReceiptBarcodeLine(int type, String data)](#meposreceiptbarcodelineint-type-string-data)
    - [MePOSReceiptFeedLine(int lines)](#meposreceiptfeedlineint-lines)
    - [MePOSReceiptImageLine(Bitmap image)](#meposreceiptimagelinebitmap-image)
    - [MePOSReceiptPriceLine(String leftText, int leftStyle, String rightText, int rightStyle)](#meposreceiptpricelinestring-lefttext-int-leftstyle-string-righttext-int-rightstyle)
    - [MePOSReceiptSingleCharLine(char chr)](#meposreceiptsinglecharlinechar-chr)
    - [MePOSReceiptTextLine(String text, int style, int size, int position)](#meposreceipttextlinestring-text-int-style-int-size-int-position)
- [Text style constants](#text-style-constants)
- [Text size constants](#text-size-constants)
- [Text position constants](#text-position-constants)
- [Contact](#contact)


## Supported tablet devices
The MePOS connect library supports Android tablets and Windows PC’s via a USB and Wi-Fi connection. Due to the
restrictions of the iOS platform it is possible to connect to the MePOS unit via Wi-Fi only from an Apple device.

## Supported MePOS firmware
The MePOS connect SDK has been tested with the latest MePOS 3.1 firmware.

## Use of the MePOS connect SDK on Windows
**Version**

The current Windows library version is 1.1. Details of the latest changes are in the release notes bundled with
the Windows SDK.

## Libraries
The MePOS connect SDK for Windows currently includes one library.
- meposconnect.dll

The MePOS connect library is provided for Windows for use with in .NET code using Microsoft Visual studio.
The MePOS connect library for windows is built using the Microsoft .NET framework 4.5.2. A future update will provide COM interoperability to allow legacy non .NET applications to use the library however as of version 1.1 this is not available.

## References
If the library has been referenced correctly in your development environment, you should be able to add the following using statement to your project without receiving errors:

```csharp
	using com.uniquesecure.meposconnect;
```

## Creating a new MePOS object

To instantiate a new class to communicate with the MePOS unit you will need to add the following code to your application:

```
MePOS mePOS = new MePOS();
```

The above code has now instantiated a MePOS object.

## MePOS SDK Methods
Once a MePOS object has been created there are several methods that can be executed that perform actions on the MePOS unit. The method names, syntax and usage are identical across the Windows and Android platform.

The MePOS methods will return 0 if successful or 1 if there is no connection to the MePOS, it is possible to query the connection state of the MePOS using the MePOSConnectionManager before a command is sent.

### void enableWifi()

Enables the Wifi module on the MePOS device.

### void disableWifi()

Disables the Wifi module on the MePOS device.

### void enableUSB()

Enables the USB ports on the MePOS device.

### void disableUSB()

Disables the USB ports on the MePOS device.

### int cashDrawerStatus()

Reads the status of the cash drawer. Possible return data are: `MePOS.CASH_DRAWER_STATUS_OPENED` or `MePOS.CASH_DRAWER_STATUS_CLOSED`.

### bool openCashDrawer()

Opens the cash drawer but validating if it's opened before. Returns true if the relay was activated, false if the cash drawer was already opened. Same as `openCashDrawer(true)`.

### bool openCashDrawer(bool validateCashDrawerStatus)

Opens the cash drawer, enabling the user to not verify if the cash drawer is already opened.

### int setDiagnosticLed(int position, int colour)

Sets the diagnostic led indicated with the color. The possible values for the position value are indicated in the `MePOSDiagnosticLEDs` class:

```csharp
	public const int LED_POWER = 0;
	public const int LED_NETWORK = 1;
	public const int LED_TABLET = 2;
	public const int LED_PED = 3;
	public const int LED_PRINTER = 8;
	public const int LED_USB1 = 9;
	public const int LED_USB2 = 10;
```

The value of the color is defined in the `MePOSColorCodes` class:

```csharp
	public const int LED_OFF = 0;
	public const int LED_GREEN = 1;
	public const int LED_RED = 2;
	public const int LED_AMBER = 3;
```

### int setCosmeticLedCol(int colour)

Sets the cosmetic led indicated with the next available colors in the `MePOSColorCodes` class:

```csharp
	public const int COSMETIC_OFF = 0;
	public const int COSMETIC_BLUE = 1;
	public const int COSMETIC_GREEN = 2;
	public const int COSMETIC_CYAN = 3;
	public const int COSMETIC_RED = 4;
	public const int COSMETIC_MAGENTA = 5;
	public const int COSMETIC_YELLOW = 6;
	public const int COSMETIC_WHITE = 7;
```

### int print(MePOSReceipt receipt, MePOSPrinterCallback callback)

Prints a pre-defined MePOS receipt using the built in receipt printer. To print a receipt, you must first create a MePOS receipt and add lines to it using the add command. This method will return 0 for success, 1 if no MePOS is connected or 2 if the printer is busy.

The below example prints a single line receipt:

```csharp
	MePOSReceipt r = new MePOSReceipt();
	r.addLine(new MePOSReceiptTextLine(“Hello World”, MePOS.TEXT_STYLE_BOLD, MePOS.TEXT_SIZE_WIDE, MePOS.TEXT_POSITION_CENTER));
	int success = mePOS.print(r);
```

If you need to be aware of the events such as starting to print, completed printing a receipt or error handling you should implement the `MePOSPrinterCallback` interface.

### int print(MePOSReceipt receipt)

Same as `print(receipt, null)`

### int printRAW(String rawData)

Sends a raw data to print. Useful if you have your own POS implementation

### int serialRAW(String rawData)

Sends a raw data to the DE9 port.

### bool printerBusy()

Asks to the MePOS if the printer is busy.

### void printConfigPage()

Prints the configuration page of the thermal printer.

### String getFWVersion()

Returns the firmware version of the MePOS

### String getSerialNumber()

Returns the board serial number of the MePOS

### MePOSConnectionManager getConnectionManager()

Gets the MePOSConnectionManager for the current MePOS instance. The connection manager can be used to set up the Wi-Fi network on the MePOS unit and query the connection state of the MePOS unit.

## MePOSConnectionManager

The MePOSConnectionManager can be used to configure the connection settings for the MePOS unit and to configure the Wi-Fi module on a MePOS unit.

### int getConnectionStatus()
Gets the current connection state of the MePOS unit:
- -1 = Not initialised
- 0 = Not connected
- 1 = Connected via USB
- 2 = Connected via Wi-Fi

### void setConnectionIPAddress(string IPAddress)

Sets the IP address on which the connection manager will look for a MePOS unit. The default IP address of the MePOS from the factory is 192.168.16.254, if the tablet is connecting to the MePOS as a client then you will not need to change this parameter unless the network settings on the MePOS unit have been changed.

### String getConnectionIPAddress()

Gets the current IP address setting for the connection manager.

### String MePOSGetAssignedIP()

Gets the current IP address from the connected MePOS unit. When connected to Wi-Fi this will be either the statically provided IP address or the DHCP network assigned IP address. In access point mode this will be the default IP address of 192.168.16.254.

### bool MePOSConnectDefault()

Sets the MePOS unit to factory settings.

### bool MePOSConnectDefault(MePOSConfigurationCallback callback)

Sets the MePOS unit to factory settings. Offers an interface to send notifications of the configuration progress.

### bool MePOSConnectEthernet(String IPAddress, String netmask)

Configure MePOS as Serial Ethernet, with the provided IP address and NetMask. If the IPAddress is equals to `MePOSConnectionManager.IP_AS_DHCP`, the configuration will use the IP provided.

### bool MePOSConnectEthernet(String ipAddress, String netmask, MePOSConfigurationCallback callback)

Same as `MePOSConnectEthernet(String IPAddress, String netmask)` but could add a callback to enable notification of configuration progress.

### bool MePOSConnectEthernet(MePOSConfigurationCallback callback);
Same as `MePOSConnectEthernet(MePOSConnectionManager.IP_AS_DHCP, null, callback)`

### bool MePOSConnectEthernet();
Same as `MePOSConnectEthernet(MePOSConnectionManager.IP_AS_DHCP, null, null)`

### bool MePOSConnectWiFi(String SSID, String IPAddress, String netmask, String encryption, String password)

Connects the MePOS unit to a WiFi network as a client. After performing a Wi-Fi connection the setConnectionIPAddress method must be called with the provided static IP address or the assigned DHCP IP address using the MePOSGetAssignedIP() method. If the MePOS unit is being used as an access point, connecting to a Wi-Fi network will switch the MePOS to becoming a Wi-Fi client and the MePOS unit will no longer be a Wi-Fi access point. It is only possible to configure the WiFi module when the MePOS unit is
plugged in via USB, a call to this method will return false if is no USB connection is found.

Valid input parameters are:

SSID – The SSID of the Wi-Fi network you are connecting to.

IPAddress – A valid static IP Address e.g. 192.168.0.100 or DHCP to request a DHCP address from the network.

Netmask – A valid network mask e.g. 255.255.255.0, if the

IPAddress parameter was DHCP this parameter will be ignored.

Encryption – The encryption type of the network you are connecting to, one of the following values NONE, WEP, WEP_OPEN, WPA_TKIP, WPA_AES, WPA2_TKIP, WPA2_AES, WPAWPA2_TKIP, WPAWPA2_AES

Password - The password for the network you are connecting to. This can be left blank or will be ignored if the
encryption type was set to NONE.

### bool MePOSConnectWiFi(String ssid, String ipAddress, String netmask, String encryption, String password, MePOSConfigurationCallback callback);
Same as `MePOSConnectWiFi(String ssid, String ipAddress, String netmask, String encryption, String password)` but adds a callback parameter to enable notification of configuration progress.

### bool MePOSSetAccessPoint(String SSID, String encryption, String password)

Sets the MePOS unit in to access point mode. When entering access point mode, the MePOS unit will create its own Wi-Fi network with the SSID, encryption and password provided. In access point mode the MePOS unit will create the IP network 192.168.16.0 and will get the IP address 192.168.16.254. Clients connecting to the
MePOS unit will be automatically assigned IP addresses via DHCP. If the MePOS unit is connected to a Wi-Fi network as a client, setting the MePOS unit in access point mode will remove the MePOS unit from any Wi-Fi networks it is connected to in client mode. It is only possible to configure the WiFi module when the MePOS
unit is plugged in via USB, a call to this method will return false if it is no USB connection is found.

SSID – The SSID of the Wi-Fi network you are creating.

Encryption – The encryption type of the network you are creating, one of the following values NONE, WEP,

WEP_OPEN, WPA_TKIP, WPA_AES, WPA2_TKIP, WPA2_AES, WPAWPA2_TKIP, WPAWPA2_AES

Password - The password for the network you are creating. This can be left blank or will be ignored if the encryption type was set to NONE.

### bool MePOSSetAccessPoint(String ssid, String encryption, String password, MePOSConfigurationCallback callback);

Same as `MePOSSetAccessPoint(String SSID, String encryption, String password)` but enables callback for notification progress.

### String getMACAddress()

Asks for the MAC address of the MePOS network unit.

### String getSSID();

Returns the SSID the MePOS is broadcasting

### String getRouterFirmware();

Returns the firmware version of the MePOS network unit.

## MePOSReceipt

The MePOS library also contains the classes to define and print a receipt using the receipt printer.

To create a new receipt, use the following code:

```csharp
	MePOSReceipt r = new MePOSReceipt();
```

After the receipt has been initialised you can add lines to the receipt for printing. There are currently six types of line available to print on a receipt, these are detailed below.

### setCutType(int cutType)

Specifies whether to perform a full or partial cut at the end of the receipt where the cut type is:

- `MePOS.CUT_TYPE_FULL`
- `MePOS.CUT_TYPE_PARTIAL`

Once the receipt has been created you can modify how the printer will cut the receipt after it has finished printing. As a default the printer is set to perform a full receipt cut.


### MePOSReceiptBarcodeLine(int type, String data)
The barcode line can be used to add a barcode to a receipt. There are currently three supported barcode types, UPC-A, Code 39 and PDF417. They are specified using the following constants:

- `MePOS.BARCODE_TYPE_UPCA`
- `MePOS.BARCODE_TYPE_CODE39`
- `MePOS.BARCODE_TYPE_PDF417`

The following example shows how to add a barcode to a receipt:

```csharp
	MePOSReceipt r = new MePOSReceipt();
	r.AddLine(new MePOSReceiptBarcodeLine(MePOS.BARCODE_TYPE_PDF417, “Hello World!”);
```

### MePOSReceiptFeedLine(int lines)
The feed line can be used to add whitespace to a receipt. The parameter supplied is the number of lines to feed.

The following example shows how to feed 10 lines on a receipt:

```csharp
	MePOSReceipt r = new MePOSReceipt();
	r.AddLine(new MePOSReceiptFeedLine(10);
```

### MePOSReceiptImageLine(Bitmap image)

The image line can be used to print black and white raster graphics to the printer. The bitmap provided must be
a valid bitmap.

The following example shows how to add an image to a receipt:

```csharp
	MePOSReceipt r = new MePOSReceipt();
	r.AddLine(new MePOSReceiptImageLine(bitmap);
```

### MePOSReceiptPriceLine(String leftText, int leftStyle, String rightText, int rightStyle)

The price line can be used to add a line to a receipt with text on the left and right simulating the common price item layout of many receipts. The price line takes parameters defining the text on the left and right and also the style of the text. Text styles are discussed later in this document.

The following example shows how to add a price line to a receipt:

```csharp
	MePOSReceipt r = new MePOSReceipt();
	r.AddLine(new MePOSReceiptPriceLine(“Some Item”, MePOS.TEXT_STYLE_NONE, “Some Price”, MePOS.TEXT_STYLE_NONE);
```

### MePOSReceiptSingleCharLine(char chr)

The single character line can be used to fill a single line with the same character. The parameter is the character to repeat across the whole line.

The following example shows how to add a single character to a receipt:

```csharp
	MePOSReceipt r = new MePOSReceipt();
	r.AddLine(new MePOSReceiptSingleCharLine(‘.’);
```

## MePOSReceiptTextLine(String text, int style, int size, int position)

The text line can be used to put text on a receipt on the left centre or right in different sizes or styles. The first
parameter is the text to print, the size and position constants are discussed later in this document.

The following example shows how to add a text line to a receipt:

```csharp
	MePOSReceipt r = new MePOSReceipt();
	r.AddLine(new MePOSReceiptTextLine(“Hello World!”,
	MePOS.TEXT_STYLE_NONE, MePOS.TEXT_SIZE_NORMAL,
	MePOS.TEXT_POSITION_CENTER);
```

## Text style constants
Some of the printer line commands accept a style parameter. This parameter can be any one of the following constants:

- `MePOS.TEXT_STYLE_NONE`
- `MePOS.TEXT_STYLE_BOLD`
- `MePOS.TEXT_STYLE_ITALIC`
- `MePOS.TEXT_STYLE_UNDERLINED`
- `MePOS.TEXT_STYLE_INVERSE`

Text styles can also be combined using the or operator to achieve a mix of styles, for example to print bold italic text you would use the following:

- `MePOS.TEXT_STYLE_BOLD | MePOS.TEXT_STYLE_ITALIC`

## Text size constants

Some of the printer line commands accept a size constant. This can be one of the following but not both:

- `MePOS.TEXT_SIZE_NORMAL`
- `MePOS.TEXT_SIZE_WIDE`

## Text position constants

Some of the printer line commands accept a position constant. This can be any of the following:

- `MePOS.TEXT_POSITION_LEFT`
- `MePOS.TEXT_POSITION_CENTER`
- `MePOS.TEXT_POSITION_RIGHT`

## Contact

Please rise a ticket on [MePOS Support](https://mepos.zendesk.com/hc/en-us/requests/new)
