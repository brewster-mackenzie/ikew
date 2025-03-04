# Add JSON serialisaton support to C# types

#csharp #json #text

-----

Use the following as a rough guide to add JSON serialisaton support 
to C# types. 

```csharp
// Additional imports

using System.Text.Json;
using System.Text.Json.Serialization;

// Some example types

public enum Status
{
  Disabled,
  Enabled
}

public class User
{
  [JsonPropertyName("username")]
  public string UserName { get; set; }
}

public class Subscription
{
  [JsonPropertyName("subId")]
  public string Id { get; set; }

  [JsonPropertyName("status")]
  public Status Status { get; set; }

  [JsonPropertyName("history")]
  public List<User> Users { get; set; }
}
```

Then you can serialize the objects using default serialization options:

```csharp 
static void Example()
{
  // Create an object 
  var sub = new Subscription();
  sub.Id = "SUB-TEST-001";
  sub.Status = Status.Enabled;
  sub.Users = new();
  sub.Users.Add(new User() {
    UserName = "Administrator"
  });

  // Serialize 
  JsonSerializer.Serialize<Subscription>(sub);

  // Deserialize
  JsonSerializer.Deserialize<Subscription>(sub);
}
```

Will give you output like this:

```json 
{
  "subId" : "SUB-TEST-001",
  "status" : 1,
  "users" : [ 
    { "username" : "Administrator" }
  ]
}
```
  
To use the text value of an `enum` instead of its numeric value:

```csharp 
// Create the appropriate options
var opts = new JsonSerializerOptions();
opts.Converters.Add(new JsonStringEnumConverter());

// Serialize 
JsonSerializer.Serialize<Subscription>(sub, opts);

// Deserialize 
JsonSerializer.Deserialize<Subscription>(sub, opts);
```

For ease of use, try baking the options and generic methods into a wrapper
class with a single generic argument.  This is particularly helpful when 
working with a known list of types in PowerShell, where invoking generic 
methods can be clumsy and difficult to read.

```csharp
public static class JsonSerializer<T> 
{
    public static T Deserialize(string json) 
    {
        var opts = new JsonSerializerOptions();
        opts.Converters.Add(new JsonStringEnumConverter());
        return JsonSerializer.Deserialize<T>(json, opts);
    }

    public static string Serialize(T value)
    {
        var opts = new JsonSerializerOptions();
        opts.Converters.Add(new JsonStringEnumConverter());
        return JsonSerializer.Serialize<T>(value, opts);
    }
}
```

This is also helpful when you want to infer the type from a string inside
a PowerShell script, since again this can be tricky when accessing the methods
directly from the shell.

```powershell 
function Get-DataFromApi ($Path, $ResponseType) {
  # Assume we are calling an API with a known response type 
  $res = Invoke-WebRequest -Uri "https://my.test/api/$Path" -Method GET -ContentType application/json

  # Create an instance of the serializer using this response type
  $inst = [Type]"JsonSerializer[$ResponseType]"
  
  # Deserialize 
  $inst::Deserialize($res.Content)
}

# Example usage 

Get-DataFromApi 'customers' 'CustomerDetails'

Get-DataFromApi 'suppliers' 'SupplierDetails'
```
