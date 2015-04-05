# Creational

## Factory Method (Class Scope)

Let subclasses decide which objects to instantiate.

### Players

* Creator: Defines a method that needs to create an object whose actual type is unknown. Does so using abstract-method call.
* Concrete Creator: Subclass that overrides the abstract object-instantiation method to create the Concrete Product.
* Product: Interface implemented by the created product. Creator accesses the Concrete Product object through this interface.
* Concrete Product: Object used by the Creator (superclass) methods. Implements the Product interface.

### Example

```ruby
class Pond # Creator
  def initialize(number_animals)
    @animals = []
    number_animals.times do |i|
    animal = new_animal("Animal#{i}")
      @animals << animal
    end
  end

  def simulate_one_day
    # Examples of the Product protocol
    @animals.each {|animal| animal.speak}
    @animals.each {|animal| animal.eat}
    @animals.each {|animal| animal.sleep}
  end 
end

class DuckPond < Pond # Concrete Creator
  def new_animal(name)
    Duck.new(name) # Concrete Product
  end
end

class FrogPond < Pond  # Concrete Creator
  def new_animal(name)
    Frog.new(name) # Concrete Product
  end
end
```
## Abstract Factory

Create objects knowing only the interfaces they implement (without knowing the actual class). Typically, create one of a â€œfamilyâ€ of objects (one of several kinds of Iterators, one of several kinds of graphical widgets, and so on).

### Players

* Abstract Factory: Interface to the actual factory.
* Concrete Factory: Implements the Abstract Factory interface to create a specific class of object.
* Abstract Product: The sort of product that the Abstract Factory creates.
* Concrete Product: The actual object (whose class you don't know) created by the factory.
* Client: Uses the created objects only through their interfaces.

### Example 1

```ruby
home = URL.new("http://www.firsthand.ca") # concrete URLConnection factory
connection = home.get_connection() # concrete URLConnection product and an abstract InputStream factory
input_stream = connection.get_input() # concrete InputStream product
```

### Example 2

```ruby
class Habitat # client
  def initialize(number_animals, number_plants, organism_factory)
    @organism_factory = organism_factory
    @animals = []
    number_animals.times do |i|
      animal = @organism_factory.new_animal("Animal#{i}")
        @animals << animal
    end
    @plants = []
    number_plants.times do |i|
      plant = @organism_factory.new_plant("Plant#{i}")
        @plants << plant
    end
  end

  # rest of class...
end

class OrganismFactory # Abstract Factory
  def initialize(plant_class, animal_class)
    @plant_class = plant_class
    @animal_class = animal_class
  end

  def new_animal(name)
    @animal_class.new(name)
  end

  def new_plant(name)
    @plant_class.new(name)
   end 
end

jungle_organism_factory = OrganismFactory.new(Tree, Tiger) # Concrete Factory and Concrete Products
pond_organism_factory = OrganismFactory.new(WaterLily, Frog) # Concrete Factory and Concrete Products

jungle = Habitat.new(1, 4, jungle_organism_factory)
jungle.simulate_one_day

pond = Habitat.new( 2, 4, pond_organism_factory)
pond.simulate_one_day 
```

## Singleton

(got it)

# Structural

## Composite

Organize a runtime hierarchy of objects that represent container/content (or whole/part) relation- ships as a collection of objects that implement a common interface. Some of the implementers of this interface define stand-alone objects, and others define containers that can hold additional objects, including other containers.

### Players

* Component: An interface or abstract class that represents all objects in the hierarchy.
* Composite: A Component that can hold other Components. It doesnâ€™t know whether these subcomponents are other Composites or are Leaves.
* Leaf: A Component that stands alone; it cannot contain anything.

### Example

```ruby
class SimpleFile # Leaf and Component
  def open; end
  def close; end
  def print; end # Example
end

class Directory < SimpleFile # Composite
  def initialize
    @files = []
  end
  
  def print # Example
    @files.each { |f| f.print }
  end
  def add(file); end
  def remove(file); end
  def contents; end
end
```

## Decorator

(got it)

# Behavioural

## Command

(got it)

## Observer (Publish/Subscribe)

(got it)

## Template Method

(got it)

## Vistor

Add operations to a â€œhostâ€ object by providing a way for a visitor â€” an object that encapsulates an algorithm â€” to access the interior state of the host object. Typically, this pattern is used to interact with elements of an aggregate structure. The visitor moves from object to object within the aggregate.

### Players

* Visitor: Defines an interface that allows access to a Concrete Element of an Object Structure. Various methods can access elements of different types.
* Concrete Visitor: Implements an operation to be performed on the Elements. This object can store an algorithm's local state.
* Element: Defines an 'accept' operations that permits Visitor to access it.
* Concrete Element: Implements the 'accept' operation.
* Object Structure: A composite object that can enumerate its elements.

### Example with one element

```ruby
class Money
  def initialize(value=0)
    @value = value
  end
  def increment(addend)
    @value += addend.value
  end
  
  def operate(visitor)
    @value = v.modify(value)
  end
end

class FutureValue
  def initialize(interest, period)
  end
  
  def modify(currentValue)
    # return future value of currentValue
  end
end

presentValue = Money.new(100)
money.operate(FutureValue.new(0.5, 24))
```

### Example with multiple elements

```ruby
class CarElement
  def accept(visitor)
    raise NotImpelementedError.new
  end
end

module Visitable
  def accept(visitor)
    visitor.visit(self)
  end
end

class Wheel < CarElement
  include Visitable

  attr_reader :name
  def initialize(name)
    @name = name
  end
end

class Engine < CarElement
  include Visitable
end

class Body < CarElement
  include Visitable
end

class Car < CarElement
  def initialize
    @elements = []
    @elements << Wheel.new("front left")
    @elements << Wheel.new("front right")
    @elements << Wheel.new("back left")
    @elements << Wheel.new("back right")
    @elements << Body.new
    @elements << Engine.new
  end

  def accept(visitor)
    @elements.each do |element|
      element.accept(visitor)
    end
    visitor.visit(self)
  end
end

class CarElementPrintVisitor
  def visit(subject)
    puts "Visiting: %s" % subject.class.to_s
  end
end

class CarElementDoVisitor
  def visit(subject)
    puts "Do some other visitation: %s" % subject.class.to_s
  end
end

c = Car.new
c.accept(CarElementPrintVisitor.new)
c.accept(CarElementDoVisitor.new)
```

### Another Example

```ruby
class Directory
  class Element
    def initialize(file)
      @file = file
    end
  end
  class DirectoryElement < Element
    def accept(visitor, depth)
      visitor.visit_directory(@file, depth, f.list_files)
    end
  end
  
  class FileElement < Element
    def accept(@visitor, depth)
      visitor.visit_file(file, depth)
    end
  end
  
  def initialize(root)
    @root = File.new(root)
  end
  
  def traverse(visitor)
    top_down(root, visitor, 0)
  end
  
  private
  
  def top_down(root, visitor, depth)
    element = root.file?
      ? FileElement.new(root)
      : DirectoryElement.new(root)
      
    element.accept(visitor, depth)
    
    if !root.file?
      children = root.list_files
      children.each do |child|
        top_down(child, visitor, depth + 1)
      end
    end
  end
end

class DirectoryPrinter
  def visit_file(file, depth)
    # noop
  end
  
  def visit_directory(file depth, children)
    depth.downto(0) { print ".." }
    puts f.get_name
  end
end

directory = Directory.new("c:/")
d.traverse(DirectoryPrinter.new)
```