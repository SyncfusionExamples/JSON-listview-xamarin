# How to bind JSON data to xamarin.Forms ListView (SfListView) ?
You can bind the data from JSON (JavaScript Object Notation) file in Xamarin.Forms SfListView using the ItemsSource property.

**XAML**

The JSON data can be bound to the SfListView ItemsSource property.
``` xml
<syncfusion:SfListView x:Name="listView" ItemSize="70" 
                        ItemSpacing="5" Grid.Row="1"
                        ItemsSource="{Binding ItemsSource}">
    <syncfusion:SfListView.ItemTemplate>
        ...
    </syncfusion:SfListView.ItemTemplate>
</syncfusion:SfListView>
```
**C#**

Accessed the JSON file from local folder and StreamReader reads the data to return as a dynamic object.
``` c#
public ViewModel()
{
    var assembly = typeof(MainPage).GetTypeInfo().Assembly;
    Stream stream = assembly.GetManifestResourceStream( 
                    "ListViewXamarin.Data.Data.json");
    using (StreamReader sr = new StreamReader(stream))
    {
        var jsonText = sr.ReadToEnd();
        ItemsSource = JsonConvert.DeserializeObject<dynamic>(jsonText);
    }
}
```
