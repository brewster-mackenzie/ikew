# Get locale-specific 'Yes' and 'No' strings

#csharp #i18n

-----

Get the locale-specific 'Yes' and 'No' strings for the user's current language


## C#

```csharp
using System;
using System.Text;
using System.Runtime.InteropServices;

public class YesNoText {
    [DllImport("user32.dll", CharSet = CharSet.Auto)]
    static extern int LoadString(IntPtr hInstance, uint uID, StringBuilder lpBuffer, int nBufferMax);
    [DllImport("kernel32")]
    static extern IntPtr LoadLibrary(string lpFileName);

    static YesNoText() {
        StringBuilder sb = new StringBuilder(256);    
        IntPtr user32 = LoadLibrary(Environment.SystemDirectory + "\\User32.dll");
        LoadString(user32, 805, sb, sb.Capacity);
        YES = sb.ToString();
        LoadString(user32, 806, sb, sb.Capacity);
        NO = sb.ToString();
    }               

    public static string YES;
    public static string NO;
}
```


