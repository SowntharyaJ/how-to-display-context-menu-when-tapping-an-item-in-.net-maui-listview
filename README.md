# how-to-display-context-menu-when-tapping-an-item-in-.net-maui-listview

This repository contains a sample demonstrating how to display context menu with .NET MAUI Popup (SfPopup) when tapping an item in .NET MAUI ListView (SfListView).

## Sample

```xaml
<syncfusion:SfListView
            x:Name="listView"
            ItemSize="75"
            ItemsSource="{Binding ContactsInfo}"
            SelectionMode="Single">
            <syncfusion:SfListView.Behaviors>
                <local:Behavior />
            </syncfusion:SfListView.Behaviors>
            <syncfusion:SfListView.ItemTemplate>
                <DataTemplate>
                    <Grid VerticalOptions="Center">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="1" />
                        </Grid.RowDefinitions>
                        <Grid Grid.Row="0">
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="50" />
                                <ColumnDefinition Width="*" />
                            </Grid.ColumnDefinitions>
                            <Image
                                Grid.Column="0"
                                HeightRequest="50"
                                Source="{Binding ContactImage}"
                                WidthRequest="50" />
                            <StackLayout
                                Grid.Column="1"
                                HorizontalOptions="StartAndExpand"
                                Orientation="Vertical"
                                VerticalOptions="Center">
                                <Label
                                    HorizontalOptions="Center"
                                    HorizontalTextAlignment="Center"
                                    LineBreakMode="WordWrap"
                                    Text="{Binding ContactName}"
                                    TextColor="#474747"
                                    VerticalOptions="Center"
                                    VerticalTextAlignment="Center" />
                                <Label
                                    HorizontalOptions="Center"
                                    HorizontalTextAlignment="Center"
                                    LineBreakMode="WordWrap"
                                    Text="{Binding ContactNumber}"
                                    TextColor="#474747"
                                    VerticalOptions="Center"
                                    VerticalTextAlignment="Center" />
                            </StackLayout>
                        </Grid>
                        <BoxView Grid.Row="1" />
                    </Grid>
                </DataTemplate>
            </syncfusion:SfListView.ItemTemplate>
        </syncfusion:SfListView>
```

```c#

listView.ItemLongPress += ListView_ItemHolding;
private void ListView_ItemHolding(object sender, ItemLongPressEventArgs e)
{
    item = e.DataItem as Contacts;
    var Layout = ListView.ItemsLayout as LinearLayout;
    var rowItems = Layout!.GetType().GetRuntimeFields().First(p => p.Name == "items").GetValue(Layout) as IList;

    foreach (ListViewItemInfo iteminfo in rowItems)
    {
        if(iteminfo.Element != null && iteminfo.DataItem == item)
        {
            itemView = iteminfo.Element as View;
        }
    }
    
    popupLayout = new SfPopup();
    popupLayout.HeightRequest = 200;
    popupLayout.WidthRequest = 150;
    popupLayout.ContentTemplate = new DataTemplate(() =>
    {

        var mainStack = new StackLayout();
        mainStack.BackgroundColor = Colors.Teal;
        

        var deletedButton = new Button()
        {
            Text = "Delete",
            HeightRequest=50,
            BorderWidth=1,
            BorderColor = Colors.White,
            BackgroundColor=Colors.Teal,
            TextColor = Colors.White
        };
        deletedButton.Clicked += DeletedButton_Clicked;
        var AddButton = new Button()
        {
            Text = "Add",
            HeightRequest = 50,
            BorderWidth = 1,
            BorderColor = Colors.White,
            BackgroundColor = Colors.Teal,
            TextColor = Colors.White,
            Command = (ListView.BindingContext as ContactsViewModel).AddCommand
            
        };
        var Sortbutton = new Button()
        {
            Text = "Sort",
            BorderWidth = 1,
            HeightRequest = 50,
            BorderColor = Colors.White,
            BackgroundColor = Colors.Teal,
            TextColor = Colors.White,
        };
        Sortbutton.Clicked += Sortbutton_Clicked;
        var Dismiss = new Button()
        {
            Text = "Cancel",
            HeightRequest = 50,
            BorderWidth = 1,
            BorderColor = Colors.White,
            BackgroundColor = Colors.Teal,
            TextColor = Colors.White,
        };
        Dismiss.Clicked += Dismiss_Clicked;
        mainStack.Children.Add(deletedButton);
        mainStack.Children.Add(AddButton);
        mainStack.Children.Add(Sortbutton);
        mainStack.Children.Add(Dismiss);
        return mainStack;

    });

    popupLayout.PopupStyle.CornerRadius = 5;
    popupLayout.ShowHeader = false;
    popupLayout.ShowFooter = false;
    popupLayout.ShowRelativeToView(itemView, PopupRelativePosition.AlignBottomRight);
}
```

## Requirements to run the demo

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) or [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)
* Xamarin add-ons for Visual Studio (available via the Visual Studio installer).

## Troubleshooting

### Path too long exception

If you are facing path too long exception when building this example project, close Visual Studio and rename the repository to short and build the project.
