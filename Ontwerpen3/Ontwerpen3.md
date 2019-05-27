# Adaptor

Het Adaptor Pattern converteert de interface van een klasse naar een andere interface die de client verwacht. Adapters zorgen ervoor dat klassen samenwerken. Zonder de adapters lukt dit niet vanwege incompatibele interfaces.

![image](./images/Adaptor.png)

# Builder

Gebruik het builder Pattern om de constructie van een product af te schermen en zorg dat je het in stappen kan construeren.

* De **Builder** klasse specifieert een abstracte interface voor de creatie van de onderdelen van het Product object. De Builder bevat alle methodes die nodig zijn voor het bouwen van de onderdelen.
* De **ConcreteBuilder** bouwt de onderdelen van het complexe object en gooit deze samen door implementatie van de Builder Interface. Het houdt een representatievan het object bij en biedt een interface voor het opvangen van het product.
* De **Director** klasse bouwt het complexe object gebruik makend van de interface van de Builder. De Director ken de stappen voor het bouwen en vraagt de Builder om de structuur te maken.
* Het **Product** stelt het complexe object voor dat gebouwd wordt.

Voordelen: 

* Schermt de manier waarop een complex object gebouwd wordt af.
* Geeft de mogelijkheid om objecten in meerdere stappen en wisselende processen te maken, in tegenstelling tot éénstapsfactory.
* Verbergt interne representatie van het product voor de Client.
* Productimplementaties kunnen steeds wisselen, omdat een client alleen een abstracte interface ziet.



![image](./images/Builder.png)
![image](./images/Builder1.png)

```java
// De builder: de abstracte klasse
public abstract class SandwichBuilder {
    private Sandwich sandwich;
    public Sandwich getSandwich {
        return sandwich;
    }    

    public void createNewSandwich() {
        sandwich = new Sandwich();
    }

    public abstract void prepareBread();
    public abstract void applyMeatAndCheese();
    public abstract void applyVegetables();
    public abstract void addCondiments();
}

// De builder klassen: concrete klassen
public class MySandwhichBuilder extends SandwichBuilder {
    public void prepareBread() {
        Sandwich sandwich = getSandwich();
        sandwich.setbreadType(BreadType.Wheat);
    }
    public void applyMeatAndCheese() {
        // ...
    }
    public void applyVegetables() {
        // ...
    }
    public void addCondiments() {
        // ...
    }
}

public class ClubSandwichBuilder extends SandwichBuilder {
    public void prepareBread() {
        Sandwich sandwich = getSandwich();
        sandwich.setbreadType(BreadType.White);
    }
    public void applyMeatAndCheese() {
        // ...
    }
    public void applyVegetables() {
        // ...
    }
    public void addCondiments() {
        // ...
    }
}

// De Director
public class SandwichDirector {
    private SandwichBuilder builder;

    public SandwichDirector(SandwichBuilder builder) {
        this.builder = builder;
    }

    public void buildSandwich() {
        builder.createNewSandwich();
        builder.prepareBread();
        builder.applyMeatAndCheese();
        builder.applyVegetables();
        builder.addCondiments();
    }

    public Sandwich getSandwich() {
        return builder.getSandwich();
    }
}

public static void main(String[] args) {
    SandwichDirector director = new SandwichDirector(new MySandwhichBuilder());

    director.buildSandwich();
    Sandwich sandwich = director.getSandwich();
    sandwich.display();
}
```

# Command

het Command Pattern schermt een aanroep af door middel van een object, waarbij je verschillende aanroepen in verschillende objecten kan opbergen, in een queue zetten, of op schijf kunt bewaren. Ook undo-operaties kunnen worden ondersteund.

* Een actie wordt voorgesteld als een **object**.
* De commandszijn volledig **self-contained**.
* Nieuwe commandos kunnen eenvoudig worden toegevegd.

![image](./images/Command.png)
![image](./images/Command1.png)

## Macro-Command

![image](./images/Command-Macro.png)

# Composite

Het Composite Pattern stelt je in staat om objecten in boomstructuren samen te stellen om partwhole hiërarchiën weer te geven. Composite laat clients de afzonderlijke objecten of samengestelde objecten op uniforme wijze behandelen.

* De **Client** gebruikt de Component interface om de objecten in de compositie te manipuleren.
* De **Component** definiëert een interface voor alle objecten in de compositie (leafs en composites).
* Een **Leaf** heeft geen children en definiëert het gedrag voor elementen in de compositie.
* De **Composite** definiëert het gedrag van de componenten met kinderen, de Composite implementeert ook Leaf-gerelateerde operaties.

![image](./images/Composite.png)
![image](./images/Composite1.png)

```java
public abstract class MenuComponent {
    public void add(MenuComponent menuComponent) {
        throw new UnsupportedOperationException();
    }

    public void remove(MenuComponent menuComponent) {
        throw new UnsupportedOperationException();
    }

    public MenuComponent getChild(int i) {
        throw new UnsupportedOperationException();
    }

    public String getName() {
        throw new UnsupportedOperationException();
    }

    public String getDescription() {
        throw new UnsupportedOperationException();
    }

    public double getPrice() {
        throw new UnsupportedOperationException();
    }

    public boolean isVegetarian() {
        throw new UnsupportedOperationException();
    }

    public abstract void print();
}

public class MenuItem extends MenuComponent {
    private String name;
    private String description;
    private boolean vegetarian;
    private double price;

    public MenuItem(String name, String description, boolean vegetarian, double price) {
        this.name = name;
        this.description = description;
        this.vegetarian = vegetarian;
        this.price = price;
    }

    public String getName() { return name; }
    public String getDescription() { return description; }
    public double getPrice() { return price; }
    public boolean isVegetarian() { return vegetarian; }

    public void print() {
        System.out.print("  " + getName());
        if (isVegetarian()) {
            System.out.print("(v)");
        }
        System.out.println("," + getPrice()):
        System.out.println("  --" + getDescription()):
    }
}

public class Menu extends MenuComponent {
    private List<MenuComponent> menuComponents;
    private String name;
    private String description;

    public Menu(String name, String description) {
        this.name = name;
        this.description = description;
    }

    public void add(MenuComponent menuComponent) (
        menuComponents.add(menuComponent);
    )

    public void remove(MenuComponent menuComponent) (
        menuComponents.remove(menuComponent);
    )

    public MenuComponent getChild(int i) {
        return (MenuComponent) menuComponents.get(i);
    }

    public String getName() {
        return name;
    }

    public String getDescription() {
        return description;
    }

    public void print() {
        System.out.print("\n" + getName());
        System.out.print("," + getDescription());
        System.out.print("--------------------");

        menuComponents.forEach(MenuComponent::print);
    }
}

public class Waitress {
    private MenuComponent allMenus;

    public Waitress(MenuComponent allMenus) {
        this.allMenus = allMenus;
    }

    public void printMenu() {
        allMenus.print();
    }
}
```

## Null Iterator

In **MenuItem**: Null iterator retourneert altijd false bij aanroep van hasNext().

```java
@Override
public Iterator createIterator() {
	return new NullIterator();
}
```

De **NullIterator** klasse:

```java
public class NullIterator implements Iterator<MenuComponent> {
    public MenuComponent next() {
        return null;
    }

    public boolean hasNext() {
        return false;
    }

    public void remove() {
	throw new UnsupportedOperationException("Not supported");
    }
}
```

## Composite Iterator

In **Menu**: een knoop geeft een iterator terug over zijn kinderen.

```java
@Override
public Iterator createIterator() {
	if (iterator == null) {
		itarator = new CompositeIterator(menuComponents.iterator());
	}
	return iterator;
}
```

De **CompositeIterator** klasse: 

```java
public class CompositeIterator implements Iterator<MenuComponent> {
    private Stack<Iterator<MenuComponent>> stack = new Stack<>();

    public CompositeIterator(Iterator<MenuComponent> iterator)
    {
        stack.push(iterator);
    }

    public MenuComponent next()
    {
        if (hasNext()) {
            Iterator<MenuComponent> iterator = stack.peek();
            MenuComponent component = iterator.next();

            // It is not a leaf, it has children
            if (component instanceof Menu) {
                stack.push(component.createIterator());
            }
        }

        return null;
    }

    public boolean hasNext()
    {
        if (stack.empty()) return false;

        Iterator<MenuComponent> iterator = stack.peek();

        if ( ! iterator.hasNext()) {
            stack.pop();
            return hasNext();
        }

        return true;
    }
}
```

# Abstract Factory

Het Abstract Factory Pattern levert een interface voor de vervaardiging van reeksen gerelateerde of onafhankelijke objecten zonder hun concrete klassen te specifieren.

* De **Abstract Factory** definieert de interface dat alle Concrete Factories moeten implementeren, die bestaat uit een set van methodes voor de creatie van producten.
* De **Concrete Factory** maakt eigen versie van elk Product. De Client gebruikt een van deze Factories zodat die nooit een Product object moet instantieren.
* De **Client** is geschreven met de Abstracte Factory en wordt tijdens runtime composed met een Concrete Factory.

We schrijven de code zodanig dat de **Client** de **Factory** gebruikt voor het maken van de producten. Door een verscheidenheid aan fabrieken krijgen we een verscheidenheid aan implementaties voor de producten. Maar de Clientcode blijft hetzelfde.

We voorzien een manier voor het maken van een familie producten, Code is losgekoppeld van de Factory waardoor een variatie van Factories kunnen geimplementeerd worden.

![image](./images/AbstractFactory.png)

```java
public interface PizzaIngredientFactory {

	public Dough createDough();
	public Sauce createSauce();
	public Cheese createCheese();
	public Veggies[] createVeggies();
	public Pepperoni createPepperoni();
	public Clams createClam();

}

public class NYPizzaIngredientFactory implements PizzaIngredientFactory {
	
	public Dough createDough() {
		return new ThinCrustDough();
	}

	public Sauce createSauce() {
		return new MarinaraSauce();
	}

	public Cheese createCheese() {
		return new ReggianoCheese();
	}

	public Veggies[] createVeggies() {
		Veggies veggies[] = { new Garlic(), new Onion(), new RedPepper() };
		return veggies;
	}

	public Pepperoni create Pepperoni() {
		return new SlicedPepperoni();
	}

	public Clams createClam() {
		return new FreshClams();
	}
}

public abstract class Pizza {

	String name;
	Dough dough;
	sauce sauce;
	Veggies veggies[];
	Cheese cheese;
	Pepperoni pepperoni;
	Clams clam;

	abstract void prepare();

	void bake() {
		System.out.println("Bake for 25 minutes at 350°");
	}

	void cut() {
		System.out.println("Cutting the pizza into diagonal slices");
	}

	void box() {
		System.out.println("Place pizza in official PizzaStore box");
	}

	void setName(String name) {
		this.name = name;
	}

	String getName() {
		return name;
	}
}

public class CheesePizza extends Pizza {
	PizzaIngredientFactory ingredientFactory;

	public CheesePizza(PizzaIngredientFactory ingredientFactory) {
		this.ingredientFactory = ingredientFactory;
	}

	void prepare() {
		System.out.println("Preparing " + name);
		dough = ingredientFactory.createDough();
		sauce = ingredientFactory.createSauce();
		cheese = ingredientFactory.createCheese();
	}
}

public class NYPizzaStore extends PizzaStore {

	protected Pizza createPizza(String item) {
		Pizza pizza = null;
		PizzaIngredientFactory ingredientFactory = new PizzaIngredientFactory();

		if (item.equals("cheese")) {
			pizza = new CheesePizza(ingredientFactory);
			pizza.setName("New York Style Cheese Pizza");
		} else if (item.equals("veggie")) {
			pizza = new VeggiePizza(ingredientFactory);
			pizza.setName("New York Style Veggie Pizza");
		} else if (item.equals("clam")) {
			pizza = new ClamPizza(ingredientFactory);
			pizza.setName("New York Style Clam Pizza");
		} else if (item.equals("pepperoni")) {
			pizza = new PepperoniPizza(ingredientFactory);
			pizza.setName("New York Style Pepperoni Pizza);
		}
		return pizza;
	}
}
```

# Factory Method

Het Factory Method Pattern definieert een interface voor het creëren van een object, maar laat de subklassen beslissen welke klasse er geïnstantieert wordt. De Factory Method draagt de instanties door aan de subklassen.

* De **Creator** 
	* Bevat de implementaties voor alle methodes voor manipulatie van het product.
	* Bevat abstracte Factory Methode die alle subklassen moeten implementeren.
* De **Concrete Creator** 
	* Implementeert de Factory Methode.
	* Is verantwoordelijk voor de creatie van Concrete Producten.
	* Is de enige klasse die kennis over de creatie heeft.
* Alle **Producten** moeten dezelfde interface implementeren zodat de klassen die de producten gebruiken hier naar kunnen refereren.

De **Abstracte Creator-Klasse** definieert een abstracte fabrieksmethode die door de subklassen geïmplementeerd wordt om producten te vervaardigen. 

* De verantwoordelijkheid voor het maken van de producten is verplaatst naar een methode die zich gedraagt als een Factory.
* De methode isoleert de Client van het weten welk soort concreet product gemaakt is.

![image](./images/FactoryMethod.png)
![image](./images/FactoryMethod1.png)

```java
public abstract class PizzaStore {

	public Pizza orderPizza(String type) {
		Pizza pizza;

		pizza = createPizza(type);

		pizza.prepare();
		pizza.bake();
		pizza.cut();
		pizza.box();

		return pizza;
	}

	abstract Pizza createPizza(String type);
}

public class NYPizzaStore extends PizzaStore {
	
	Pizza createPizza(String item) {
		if (item.equals("cheese")) {
			return new NYStyleCheesePizza();
		} else if (item.equals("veggie")) {
			return new NYStyleVeggiePizza();
		} else if (item.equals("clam")) {
			return new NYStyleClamPizza();
		} else if (item.equals("pepperoni")) {
			return new NYStylePepperoniPizza();
		} else return null;
	}
}
```

# Simple Factory

We nemen de code voor de creatie op en verplaatsen deze naar een ander object dat alleen maar het maken als taak zal hebben. Wanneer een methode een klasse retoruneert uit verschillende klassen met gelijkaardige superklasse.

* De **Client** gaat door de Factory voor instanties van het product.
* De **Factory** maakt instanties van het product, dit is het enige deel van de applicatie die refereert daar de concrete klassen van het product.
* De **product** klasse is abstract en heeft implementaties die overschreven kunnen worden.
* de **Concrete Producten** implementeren de Product klasse en kunnen gemaakt worden door de Factory.

![image](./images/SimpleFactory.png)

```java
public class SimplePizzaFactory {

	public Pizza createPizza(String type){
		Pizza pizza = null;

		if (type.equals("cheese")) {
			pizza = new CheesePizza();
		} else if (type.equals("pepperoni")) {
			pizza = new PepperoniPizza();
		} else if (type.equals("clam")) {
			pizza = new ClamPizza();
		} else if (type.equals("veggie")) {
			pizza = new VeggiePizza();
		}
		return pizza;
	}
}
```


# Iterator

Het Iterator Pattern voorziet ons van een manier voor sequentiële toegang tot de elementen van een aggregaatobject zonder de onderliggende representatie weer te geven.

* Iterator Pattern is gebaseerd op een Iterator interface.
* Een implementatie van Iterator weet hoe hij moet itereren door zijn specifieke lijst.
* De Client wordt losgekoppeld van de implementatie van de concrete klassen.

![image](./images/Iterator1.png)
![image](./images/Iterator2.png)

```java
public interface Iterator {
	boolean hasNext();
	Object next();
}

public class DinnerMenuIterator implements Iterator {
	MenuItem[] items;
	int position = 0;

	public DinnerMenuIterator(MenuItem[] items) {
		this.items = items;
	}

	public MenuItem next() {
		MenuItem menuItem = items[position];
		position = position + 1;
		return menuItem;
	}

	public boolean hasNext() {
		if (position >= items.length || items[position] == null) {
			return false;
		} else {
			return true;
		}
	}
}

public class DinnerMenu {
	static final int MAX_ITEMS = 6;
	int nuberOfItems = 0;
	MenuItem[] menuItems;

	public Iterator createIterator() {
		return new DinnerMenuIterator(menuItems);
	}
}

public class Waitress {
	PancakeHouseMenu pancakeHouseMenu;
	DinerMenu dinerMenu;

	public Waitress(PancakeHouseMenu pancakeHouseMenu, DinerMenu dinerMenu) {
		this.pancakeHouseMenu = pancakeHouseMenu;
		this.dinerMenu = dinerMenu;
	}

	public void printMenu() {
		Iterator pancakeIterator = pancakeHouseMenu.createIterator();
		Iterator dinerIterator = dinerMenu.createIterator();

		System.out.println("MENU\n----\nBREAKFAST);
		printMenu(pancakeIterator);
		System.out.println("\nLUNCH");
		printMenu(dinerIterator);

	}

	public void printMenu(Iterator iterator) {
		while (iterator.hasNext()) {
			MenuItem menuItem = iterator.next();
			System.out.print(menuItem.getName() + ", ");
			System.out.print(menuItem.getPrice() + " -- ");
			System.out.println(menuItem.getDescription());
		}
	}
}
```

## Na opschonen met java.util.iterator en Menu Interface.

* De **PancakeHouseIterator** klasse wordt verwijderd en de create methode wordt aangepast.
* In de **DinerMenuIterator** klasse wordt een remove methode toegevoegd.

Hierdoor kent de Client enkel Menu en Iterator, de Client is losgekoppeld van de implementaties van Menus dus kan er een Iterator gebruikt worden om te itereren over elke menulijst.

![image](./images/Iterator3.png)

```java
public class PancakeHouseMenu {
	public Iterator<MenuItem> createIterator() {
		return menuItems.iterator();
	}
}

public class DinerMenuIterator implements Iterator {
	
	public void remove () {
		
		if (position <= 0) {
			throw new IllegalSateException("Kan niet verwijderen");
		}
		if (list[position-1] != null) {
			for (int i = position-1; i < (list.length-1) {
				list[i] = list[i+1];
			}
			list[list.length-1] = null;
		}
	}
}

public interface Menu {
	public Iterator<MenuItem> createIterator();
}
```
# Proxy

Het Proxy Pattern zorgt voor een surrogaat of plaatsvervanger voor een ander object om de toegang hiertoe te controleren.

Structureel gelijk aan **Decorator** maar de doelstellingen veranderen:

* Decorator voegt gedrag toe aan een object.
* Proxy regelt de toegang.

![image](./images/Proxy.png)

# Singleton

Het Singleton Pattern garandeert dat een klasse slechts één instantie heeft, en biedt een globaal toegangspunt ernaartoe.

![image](./images/Singleton1.png)

# Template Method

Het Template Method Pattern definieert het skelet van een algoritme in een methode, waarbij sommige stappen aan subklassen worden overgelaten. De Template Method laat subklassen bepaalde stappen in een algoritme herdefiniëren zonder de structuur van het algoritme te veranderen.

De Template Methode maakt gebruik van **primitieve methoden** om een algoritme te implementeren. Deze is echter ontkoppeld van de feitelijke implementatie van deze methoden.

![image](./images/Template.png)
![image](./images/Template1.png)

## Hook

![image](./images/Template-Hook.png)

// De builder: de abstracte klasse
public abstract class SandwichBuilder {
    private Sandwich sandwich;
    public Sandwich getSandwich {
        return sandwich;
    }    

    public void createNewSandwich() {
        sandwich = new Sandwich();
    }

    public abstract void prepareBread();
    public abstract void applyMeatAndCheese();
    public abstract void applyVegetables();
    public abstract void addCondiments();
}

// De builder klassen: concrete klassen
public class MySandwhichBuilder extends SandwichBuilder {
    public void prepareBread() {
        Sandwich sandwich = getSandwich();
        sandwich.setbreadType(BreadType.Wheat);
    }
    public void applyMeatAndCheese() {
        // ...
    }
    public void applyVegetables() {
        // ...
    }
    public void addCondiments() {
        // ...
    }
}

public class ClubSandwichBuilder extends SandwichBuilder {
    public void prepareBread() {
        Sandwich sandwich = getSandwich();
        sandwich.setbreadType(BreadType.White);
    }
    public void applyMeatAndCheese() {
        // ...
    }
    public void applyVegetables() {
        // ...
    }
    public void addCondiments() {
        // ...
    }
}

// De Director
public class SandwichDirector {
    private SandwichBuilder builder;

    public SandwichDirector(SandwichBuilder builder) {
        this.builder = builder;
    }

    public void buildSandwich() {
        builder.createNewSandwich();
        builder.prepareBread();
        builder.applyMeatAndCheese();
        builder.applyVegetables();
        builder.addCondiments();
    }

    public Sandwich getSandwich() {
        return builder.getSandwich();
    }
}

public static void main(String[] args) {
    SandwichDirector director = new SandwichDirector(new MySandwhichBuilder());

    director.buildSandwich();
    Sandwich sandwich = director.getSandwich();
    sandwich.display();
}
