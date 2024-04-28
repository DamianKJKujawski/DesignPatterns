# Repository: Other-Projects

  Random templates.

## Content

  - [Introduction](#Introduction)
  - [MVVM-Java](#MVVM-Java)
  - [License](#License)

## Introduction

  Templates included in the repository: 
   -  MVVM-Java

## MVVM-Java

![image](https://github.com/DamianKJKujawski/Other-Projects/assets/160174331/6362fbcc-01eb-4e1e-8bb6-430a6c32d7c1)

ViewModel:

```

public class Object2DViewModel 
{
    private Object2D person_;
    
    private StringProperty nameProperty_;
    private StringProperty widthProperty_;
    private StringProperty heightProperty_;
    
    
    public Object2DViewModel(Object2D person) 
    {
    	// Create person object:
        this.person_ = person;

        //Init StringProperty:
        this.nameProperty_ = new SimpleStringProperty(person.get_name().toString());
        
        this.widthProperty_ = new SimpleStringProperty(Integer.toString(person.get_size().get_width()));
        this.heightProperty_ = new SimpleStringProperty(Integer.toString(person.get_size().get_height()));
    }

    
    
    /* 
     * Getters and Setters: 
    */
    
    public StringProperty nameProperty() {
        return nameProperty_;
    }

    public StringProperty widthProperty() {
        return widthProperty_;
    }

    public StringProperty heightProperty() {
        return heightProperty_;
    }

    /* 
     * Data Modifiers:
    */
    
    public void updateName(String name) 
    {
        person_.set_name(name);
        nameProperty_.set(name);
    }

    public void updateWidth(int width) 
    {
        Size _currentSize = person_.get_size();
        Size _newSize = new Size(width, _currentSize.get_height());
        person_.set_size(_newSize);
        
        widthProperty_.set(Integer.toString(width));
    }
    
    public void updateHeight(int height) 
    {
        Size _currentSize = person_.get_size();
        Size _newSize = new Size(_currentSize.get_width(), height);
        person_.set_size(_newSize);
        
        heightProperty_.set(Integer.toString(height));
    }
}

```

Model:

```

public class Size 
{
    private int width_;
    private int height_;

    public Size(int width, int height) 
    {
        this.width_ = width;
        this.height_ = height;
    }

    
    
    /* 
     * Getters and Setters: 
    */

    public int get_width() {
        return width_;
    }

    public void set_width(int width) {
        this.width_ = width;
    }

    public int get_height() {
        return height_;
    }

    public void set_height(int height) {
        this.height_ = height;
    }
}

```

GUI Binder:

```

    public void start(Stage primaryStage) 
    {
        // Init: Model(Person) & ViewModel
        Object2D person = new Object2D("Skrzynka", new Size(2,2));

        // View Model:
        viewModel_ = new Object2DViewModel(person);

        // GUI
        TextField nameTextField_ = new TextField();
        TextField widthTextField_ = new TextField();
        TextField heightTextField_ = new TextField();

        // Bind Text Fields:
        nameTextField_.textProperty().bindBidirectional(viewModel_.nameProperty());
        widthTextField_.textProperty().bindBidirectional(viewModel_.widthProperty());
        heightTextField_.textProperty().bindBidirectional(viewModel_.heightProperty());

        nameTextField_.textProperty().addListener((observable, oldValue, newValue) -> {
            viewModel_.updateName(newValue);
        });

        // Text Fields:
        IntConsumer widthUpdater = value -> viewModel_.updateWidth(value);
        IntConsumer heightUpdater = value -> viewModel_.updateHeight(value);

        addIntegerValidationListener(widthTextField_, widthUpdater);
        addIntegerValidationListener(heightTextField_, heightUpdater);

        VBox root = new VBox();
        root.getChildren().addAll(new Label("Name:"), nameTextField_);
        root.getChildren().addAll(new Label("Width:"), widthTextField_);
        root.getChildren().addAll(new Label("Height:"), heightTextField_);

        // Set Scene:
        Scene _scene = new Scene(root, 300, 200);

        // Display Window:
        primaryStage.setTitle("MVVM Example");
        primaryStage.setScene(_scene);
        primaryStage.show();
    }

    // Add listener:
    private void addIntegerValidationListener(TextField textField, IntConsumer updater) 
    {
        textField.textProperty().addListener((observable, oldValue, newValue) -> 
        {
            try 
            {
                int value = Integer.parseInt(newValue);
                updater.accept(value);
            } catch (NumberFormatException e) {
                ErrorHandler.handleNumberFormatException(e, "Wprowadzona wartość musi być liczbą całkowitą!");
            }
        });
    }

```

## License

-



