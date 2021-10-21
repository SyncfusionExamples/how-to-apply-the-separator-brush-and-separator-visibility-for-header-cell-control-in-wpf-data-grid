# How to apply the SeparatorBrush and SeparatorVisibility for HeaderCellControl in WPF DataGrid(SfDataGrid)?
## About the sample

This example illustrates how to apply the SeparatorBrush and SeparatorVisibility for HeaderCellControl in [WPF DataGrid](https://www.syncfusion.com/wpf-ui-controls/datagrid) (SfDataGrid).

[WPF DataGrid](https://www.syncfusion.com/wpf-ui-controls/datagrid) (SfDataGrid) doesn’t have any direct properties for change the SeparatorColor and SeparatorVisibility of column header. But you can achieve this by customizing the `GridHeaderCellControl` style.

## Change the Columns separator color
You can change the column header separator color by change the GridSplitter background color in `GridHeaderCellControl`.

```XAML
<syncfusion:BoolToVisiblityConverter x:Key="VisiblityConverter" />
<syncfusion:SortDirectionToVisibilityConverter x:Key="sortDirectionToVisibilityConverter" />
<syncfusion:SortDirectionToWidthConverter x:Key="sortDirectionToWidthConverter" />

<Style x:Key="headerStyleSeparatorColor" TargetType="{x:Type syncfusion:GridHeaderCellControl}">
    <Setter Property="Background" Value="Transparent" />
    <Setter Property="BorderBrush" Value="Gray" />
    <Setter Property="BorderThickness" Value="0,0,0,1" />
    <Setter Property="HorizontalContentAlignment" Value="Center" />
    <Setter Property="Padding" Value="5,0,0,0" />
    <Setter Property="Foreground" Value="Gray" />
    <Setter Property="FontSize" Value="14" />
    <Setter Property="FontWeight" Value="Normal" />
    <Setter Property="IsTabStop" Value="False" />
    <Setter Property="syncfusion:VisualContainer.WantsMouseInput" Value="True" />
    <Setter Property="VerticalContentAlignment" Value="Center" />
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="{x:Type syncfusion:GridHeaderCellControl}">
                <Grid>
                    <VisualStateManager.VisualStateGroups>
                        <VisualStateGroup x:Name="HiddenColumnsResizingStates">
                            <VisualState x:Name="PreviousColumnHidden">
                                <Storyboard>
                                    <ThicknessAnimationUsingKeyFrames BeginTime="0"
                                                                  Duration="1"
                                                                  Storyboard.TargetName="PART_HeaderCellBorder"
                                                                  Storyboard.TargetProperty="BorderThickness">
                                        <EasingThicknessKeyFrame KeyTime="0" Value="3, 0, 1, 1" />
                                    </ThicknessAnimationUsingKeyFrames>
                                </Storyboard>
                            </VisualState>

                            <VisualState x:Name="HiddenState">
                                <Storyboard>
                                    <ThicknessAnimationUsingKeyFrames BeginTime="0"
                                                                  Duration="1"
                                                                  Storyboard.TargetName="PART_HeaderCellBorder"
                                                                  Storyboard.TargetProperty="BorderThickness">
                                        <EasingThicknessKeyFrame KeyTime="0" Value="3, 0, 3, 1" />
                                    </ThicknessAnimationUsingKeyFrames>
                                </Storyboard>
                            </VisualState>
                            <!--  VisualState name changed from Normal to NormalState as Normal state is used for providing MouseOver effect  -->
                            <VisualState x:Name="NormalState" />

                            <VisualState x:Name="LastColumnHidden">
                                <Storyboard>
                                    <ThicknessAnimationUsingKeyFrames BeginTime="0"
                                                                  Duration="1"
                                                                  Storyboard.TargetName="PART_HeaderCellBorder"
                                                                  Storyboard.TargetProperty="BorderThickness">
                                        <EasingThicknessKeyFrame KeyTime="0" Value="0, 0, 3, 1" />
                                    </ThicknessAnimationUsingKeyFrames>
                                </Storyboard>
                            </VisualState>
                        </VisualStateGroup>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="MouseOver" />
                            <VisualState x:Name="Normal" />
                        </VisualStateGroup>
                        <VisualStateGroup x:Name="BorderStates">
                            <VisualState x:Name="NormalCell" />
                            <VisualState x:Name="FrozenColumnCell"/>
                            <VisualState x:Name="FooterColumnCell">
                                <Storyboard BeginTime="0">
                                    <ThicknessAnimationUsingKeyFrames BeginTime="0"
                                                                  Duration="1"
                                                                  Storyboard.TargetName="PART_FooterCellBorder"
                                                                  Storyboard.TargetProperty="BorderThickness">
                                        <EasingThicknessKeyFrame KeyTime="0" Value="1,0,1,1" />
                                    </ThicknessAnimationUsingKeyFrames>
                                </Storyboard>
                            </VisualState>
                            <VisualState x:Name="BeforeFooterColumnCell">
                                <Storyboard BeginTime="0">
                                    <ThicknessAnimationUsingKeyFrames BeginTime="0"
                                                                  Duration="1"
                                                                  Storyboard.TargetName="PART_FooterCellBorder"
                                                                  Storyboard.TargetProperty="BorderThickness">
                                        <EasingThicknessKeyFrame KeyTime="0" Value="0,0,0,1" />
                                    </ThicknessAnimationUsingKeyFrames>
                                    <ThicknessAnimationUsingKeyFrames BeginTime="0"
                                                                  Duration="1"
                                                                  Storyboard.TargetName="PART_HeaderCellBorder"
                                                                  Storyboard.TargetProperty="BorderThickness">
                                        <EasingThicknessKeyFrame KeyTime="0" Value="0,0,0,1" />
                                    </ThicknessAnimationUsingKeyFrames>
                                </Storyboard>
                            </VisualState>
                        </VisualStateGroup>
                    </VisualStateManager.VisualStateGroups>
                    <Border x:Name="PART_FooterCellBorder"
                        Background="{TemplateBinding Background}"
                        BorderBrush="{TemplateBinding BorderBrush}" />
                    <Border x:Name="PART_HeaderCellBorder"
                        Background="{TemplateBinding Background}"
                        BorderBrush="{TemplateBinding BorderBrush}"
                        BorderThickness="{TemplateBinding BorderThickness}"
                        SnapsToDevicePixels="True">
                        <Grid Margin="{TemplateBinding Padding}" SnapsToDevicePixels="True">
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="*" />
                                <ColumnDefinition Width="Auto" />
                                <ColumnDefinition Width="Auto" />
                                <ColumnDefinition Width="1" />
                            </Grid.ColumnDefinitions>

                            <ContentPresenter HorizontalAlignment="{TemplateBinding HorizontalContentAlignment}"
                                          VerticalAlignment="{TemplateBinding VerticalContentAlignment}"
                                          Focusable="False" />

                            <Border x:Name="PART_FilterPopUpPresenter" />
                            <Grid x:Name="PART_SortButtonPresenter"
                              Grid.Column="1"
                              SnapsToDevicePixels="True">
                                <Grid.ColumnDefinitions>
                                    <ColumnDefinition Width="0" MinWidth="{Binding Path=SortDirection, Mode=OneWay, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource sortDirectionToWidthConverter}}" />
                                    <ColumnDefinition Width="*" />
                                </Grid.ColumnDefinitions>

                                <Path Width="8.938"
                                  Height="8.138"
                                  HorizontalAlignment="Center"
                                  VerticalAlignment="Center"
                                  Data="F1M753.644,-13.0589L753.736,-12.9639 753.557,-12.7816 732.137,8.63641 732.137,29.7119 756.445,5.40851 764.094,-2.24384 764.275,-2.42352 771.834,5.1286 796.137,29.4372 796.137,8.36163 774.722,-13.0589 764.181,-23.5967 753.644,-13.0589z"
                                  Fill="Gray"
                                  SnapsToDevicePixels="True"
                                  Stretch="Fill"
                                  Visibility="{Binding Path=SortDirection,
                                                       RelativeSource={RelativeSource TemplatedParent},
                                                       ConverterParameter=Ascending,
                                                       Converter={StaticResource sortDirectionToVisibilityConverter}}">
                                    <Path.RenderTransform>
                                        <TransformGroup>
                                            <TransformGroup.Children>
                                                <RotateTransform Angle="0" />
                                                <ScaleTransform ScaleX="1" ScaleY="1" />
                                            </TransformGroup.Children>
                                        </TransformGroup>
                                    </Path.RenderTransform>
                                </Path>

                                <Path Width="8.938"
                                  Height="8.138"
                                  HorizontalAlignment="Center"
                                  VerticalAlignment="Center"
                                  Data="F1M181.297,177.841L181.205,177.746 181.385,177.563 202.804,156.146 202.804,135.07 178.497,159.373 170.847,167.026 170.666,167.205 163.107,159.653 138.804,135.345 138.804,156.42 160.219,177.841 170.76,188.379 181.297,177.841z"
                                  Fill="Gray"
                                  SnapsToDevicePixels="True"
                                  Stretch="Fill"
                                  Visibility="{Binding Path=SortDirection,
                                                       RelativeSource={RelativeSource TemplatedParent},
                                                       ConverterParameter=Decending,
                                                       Converter={StaticResource sortDirectionToVisibilityConverter}}">
                                    <Path.RenderTransform>
                                        <TransformGroup>
                                            <TransformGroup.Children>
                                                <RotateTransform Angle="0" />
                                                <ScaleTransform ScaleX="1" ScaleY="1" />
                                            </TransformGroup.Children>
                                        </TransformGroup>
                                    </Path.RenderTransform>
                                </Path>

                                <TextBlock Grid.Column="1"
                                       Margin="0,-4,0,0"
                                       VerticalAlignment="Center"
                                       FontSize="10"
                                       Foreground="{TemplateBinding Foreground}"
                                       SnapsToDevicePixels="True"
                                       Text="{TemplateBinding SortNumber}"
                                       Visibility="{TemplateBinding SortNumberVisibility}" />
                            </Grid>

                            <syncfusion:FilterToggleButton x:Name="PART_FilterToggleButton"
                                                  Grid.Column="2"
                                                  HorizontalAlignment="Stretch"
                                                  VerticalAlignment="Stretch"
                                                  SnapsToDevicePixels="True"
                                                  Visibility="{TemplateBinding FilterIconVisiblity}" />
                            <GridSplitter Background="Red" Grid.Column="3" Width="1" IsEnabled="False"/>
                        </Grid>
                    </Border>

                </Grid>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
  

<syncfusion:SfDataGrid x:Name="sfDataGrid" 
                       AllowFiltering="True"
                       HeaderStyle="{StaticResource headerStyleSeparatorColor}" 
                       ItemsSource="{Binding Path=Orders}"
                       AutoGenerateColumns="False">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:GridTextColumn MappingName="OrderID"/>
        <syncfusion:GridTextColumn MappingName="CustomerID" />
        <syncfusion:GridTextColumn MappingName="CustomerName" />
        <syncfusion:GridTextColumn MappingName="Country" />
        <syncfusion:GridCurrencyColumn MappingName="UnitPrice"/>
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>

```
## Change the Columns separator visibility
You can change the column header separator visibility by using the `BorderThickness` property in `GridHeaderCellControl`.

```XAML
<Style x:Key="headerStyleSeparatorVisibility" TargetType="syncfusion:GridHeaderCellControl">
    <Setter Property="BorderThickness" Value="0,0,0,1"/>
</Style>

<syncfusion:SfDataGrid x:Name="sfDataGrid" 
                       AllowFiltering="True"
                       HeaderStyle="{StaticResource headerStyleSeparatorVisibility}" 
                       ItemsSource="{Binding Path=Orders}"
                       AutoGenerateColumns="False">
    <syncfusion:SfDataGrid.Columns>
        <syncfusion:GridTextColumn MappingName="OrderID"/>
        <syncfusion:GridTextColumn MappingName="CustomerID" />
        <syncfusion:GridTextColumn MappingName="CustomerName" />
        <syncfusion:GridTextColumn MappingName="Country" />
        <syncfusion:GridCurrencyColumn MappingName="UnitPrice"/>
    </syncfusion:SfDataGrid.Columns>
</syncfusion:SfDataGrid>

```

KB article - [How to apply the SeparatorBrush and SeparatorVisibility for HeaderCellControl in WPF DataGrid(SfDataGrid)?](https://www.syncfusion.com/kb/11369/how-to-apply-the-separatorbrush-and-separatorvisibility-for-headercellcontrol-in-wpf)

## Requirements to run the demo
Visual Studio 2015 and above versions
