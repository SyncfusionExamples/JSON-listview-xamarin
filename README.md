# How to bind JSON data to xamarin.Forms ListView (SfListView) ?
You can bind the data from JSON (JavaScript Object Notation) file in Xamarin.Forms SfListView using the ItemsSource property.

**XAML**

The JSON data can be bound to the SfListView ItemsSource property.
``` xml
<syncfusion:SfListView x:Name="listView" ItemSize="70" 
                        ItemSpacing="5" Grid.Row="1"
                        ItemsSource="{Binding ItemsSource}">
    <syncfusion:SfListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid>
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="*"/>
                                <ColumnDefinition Width="*"/>
                            </Grid.ColumnDefinitions>
                            <Grid.RowDefinitions>
                                <RowDefinition Height="Auto"/>
                            </Grid.RowDefinitions>
                            <Label Grid.Column="0" Text="{Binding Converter={StaticResource converter}, ConverterParameter=Value1}" HorizontalOptions="Start" TextColor="Black" FontSize="16" FontAttributes="Bold"/>
                            <Label Grid.Column="1" Text="{Binding Converter={StaticResource converter}, ConverterParameter=Value2}" HorizontalOptions="Start" TextColor="Black" FontSize="16" FontAttributes="Bold"/>
                        </Grid>
                    </ViewCell>
                </DataTemplate>
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
**C#**

Dynamic object value converted by ExpandoObject to show the values.
``` c#
    public class DynamicToPathValueConverter : IValueConverter
    {
        public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
        {
          if (value == null)
            return value;
          ExpandoObject busniessObject = JsonConvert.DeserializeObject<ExpandoObject>(value.ToString());
          var jsonList = busniessObject.ToList();
          if (parameter.Equals("Value1"))       
            return jsonList[0].Value;
          if (parameter.Equals("Value2"))
            return jsonList[1].Value;
      return value;
   }
   public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
   {
      throw new NotImplementedException();
   }
}
```
