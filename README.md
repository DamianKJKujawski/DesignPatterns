GOTO: [Base-Projects](https://github.com/DamianKJKujawski/Base-Projects) [Ideas-Projects](https://github.com/DamianKJKujawski/Ideas-Projects) [MicroOS](https://github.com/DamianKJKujawski/MicroOS) [Electronics](https://github.com/DamianKJKujawski/Electronics) [Design Patterns](https://github.com/DamianKJKujawski/DesignPatterns) [MulticorePLCUnit](https://github.com/DamianKJKujawski/MulticorePLCUnit) [PCB Design](https://github.com/DamianKJKujawski/PCB) [Antennas](https://github.com/DamianKJKujawski/Antennas)

# Repository: DesignPatterns

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



# Simple outlines:

## Singleton:

```
#include <iostream>

class Singleton
{
private:

    Singleton() {}

    static Singleton* instance;

public:

    static Singleton& getInstance()
    {
        if (!instance)
        {
            instance = new Singleton();
        }
        return *instance;
    }

    void someMethod()
    {
        std::cout << "Singletona called..." << std::endl;
    }

    Singleton(Singleton const&) = delete;
    void operator=(Singleton const&) = delete;
};

Singleton* Singleton::instance = nullptr;

int main() {

    Singleton& singleton = Singleton::getInstance();
    singleton.someMethod();
    return 0;
}
```

## MVVM:

```
#include <iostream>
#include <string>

class Model
{
  private:
      std::string data;

  public:
      void setData(const std::string& newData)
      {
          data = newData;
      }
      std::string getData() const {
          return data;
      }
};

class ViewModel
{
  private:
      Model model;
  
  public:
      void updateData(const std::string& newData)
      {
          model.setData(newData);
      }
      std::string getData() const
      {
          return model.getData();
      }
};

int main()
{
    ViewModel viewModel;

    std::cout << "Current data: " << viewModel.getData() << std::endl;

    viewModel.updateData("New data!");

    std::cout << "New Data: " << viewModel.getData() << std::endl;

    return 0;
}
```

## Factory:

```
class Product {
public:
    virtual void operation() = 0;
};

class ConcreteProductA : public Product {
public:
    void operation() override {
        std::cout << "Poduct A called" << std::endl;
    }
};

class ConcreteProductB : public Product {
public:
    void operation() override {
        std::cout << "Poduct B called" << std::endl;
    }
};


class Factory {
public:
    virtual std::unique_ptr<Product> createProduct() = 0;
};

class ConcreteFactoryA : public Factory {
public:
    std::unique_ptr<Product> createProduct() override {
        return std::make_unique<ConcreteProductA>();
    }
};

class ConcreteFactoryB : public Factory {
public:
    std::unique_ptr<Product> createProduct() override {
        return std::make_unique<ConcreteProductB>();
    }
};

int main() {

    std::unique_ptr<Factory> factoryA = std::make_unique<ConcreteFactoryA>();
    std::unique_ptr<Product> productA = factoryA->createProduct();
    productA->operation();

    std::unique_ptr<Factory> factoryB = std::make_unique<ConcreteFactoryB>();
    std::unique_ptr<Product> productB = factoryB->createProduct();
    productB->operation();

    return 0;
}
```

## Strategy Pattern:

```
#include <iostream>
#include <memory>

class Strategy {
public:
    virtual void execute() = 0;
    virtual ~Strategy() {}
};

class ConcreteStrategyA : public Strategy
{
public:
    void execute() override {
        std::cout << "Wywołano strategię A" << std::endl;
    }
};

class ConcreteStrategyB : public Strategy
{
public:
    void execute() override {
        std::cout << "Wywołano strategię B" << std::endl;
    }
};


class Context
{
private:
    std::unique_ptr<Strategy> strategy;

public:
    void setStrategy(std::unique_ptr<Strategy> newStrategy)
    {
        strategy = std::move(newStrategy); // Move resources...
    }

    void executeStrategy()
    {
        if (strategy)
        {
            strategy->execute();
        } else
        {
            std::cout << "Brak zdefiniowanej strategii" << std::endl;
        }
    }
};

int main()
{
    Context context;

    context.setStrategy(std::make_unique<ConcreteStrategyA>());
    context.executeStrategy();

    context.setStrategy(std::make_unique<ConcreteStrategyB>());
    context.executeStrategy();

    return 0;
}
```
