Download Link: https://assignmentchef.com/product/solved-oop345-workshop-6-stl-containers
<br>
In this workshop, you store polymorphic objects in an STL container.

You are going to create an application that simulates an autoshop that sells various types of vehicles.  This specific application, will focus on **cars** and **racecars**.

## *In-Lab*

The in-lab portion of this workshop consists of modules:– `w6` (supplied)– `Vehicle` (supplied)– `Car`– `Autoshop`– `Utilities`

Enclose all your source code within the `sdds` namespace and include the necessary guards in each header file.

### `Car` Module

This module defines a class that holds information about a single car.

***Private Data***

Design and code a class named `Car`, that inherits the interface `Vehicle` (supplied), and that should be able to store the following information (for each attribute, you can chose any type you think it’s appropriate–you must be able to justify the decisions you have made):

– **maker**– **condition**: a car can be **new**, **used**, or **broken** in need of repairs.– **top speed**

***Public Members***

– a custom constructor that receives an object of type `std::istream&amp;` as parameter. This constructor should be able to read a single record from the stream, extract all the information about a single car and store it in the attributes. Each record has the following format:“`TAG,MAKER,CONDITION,TOP_SPEED“`– `TAG`, is a single character representing the type of vehicle: `c` or `C` for car. Any other tag is considered invalid.– `MAKER`, the name of the company that makes the car– `CONDITION`, a single character: `n` for **new**, `u` for **used**, and `b` for **broken**. Any other character is considered invalid.– `TOP_SPEED`, an integer containing the maximum speed the vehicle can achieve. If the content of this field is not a number, then the field is considered invalid.– any space at the beginning/end of each field should be removed.– `std::string condition() const`: a query that returns `new`, `used` or `broken`, representing the condition of the car– `double topSpeed() const`– `void display(std::ostream&amp; out) const`, a query that inserts in the first parameter the content of the car object in the format `| MAKER | CONDITION | TOP_SPEED |`, where– `MAKER`, field of size 10– `CONDITION`, field of size 6– `TOP_SPEED`, field of size 6, with two significant digits after the period– see alignment in the sample output.

**Add any other function that is required by your design!**

For the in-lab portion, assume there are no invalid records in the file.

### `Autoshop` module

This module defines a class that holds information about an autoshop that sells various types of vehicles.

***Private Data***

Design and code a class named `Autoshop` that should be able to store the following information:

– `std::vector&lt;Vehicle*&gt; m_vehicles`: a vector that store all the vehicles available at this autoshop.

***Public Members***– `Autoshop&amp; operator +=(Vehicle* theVehicle)`: adds the vehicle received as parameter into the `m_vehicles` vector.– `void display(std::ostream&amp; out) const`: iterates over the vehicles stored in `m_vehicles` and prints them into the parameter (use `Vehicle::display()`) using the format:“`——————————–| Cars in the autoshop!        |——————————–VEHICLEVEHICLE…——————————–“`

**To iterate over the elements of the vector, use STL iterators!**

### `Utilities` module

This module should contain a single function that creates instances on the `Vehicle` hierarchy:“`Vehicle* createInstance(std::istream&amp; in);“`

This function should extract data from the parameter; if the first non-blank character is `c` or `C`, the this function should **dynamically** create an instance of type `Car` passing the stream to the constructor, and return it to the client.

If the first non-blank character is anything else (or there are no more characters to extract), this function should return `nullptr`.

Because the input file contains two types of delimiters (`
` for records, and `,` for the fields in a record), you can use the class `std::stringstream` (utilization of this class is not mandatory, the extraction can be achieved without using it).

When implementing the `createInstance` function, consider the following STL class:– [std::stringstream](https://en.cppreference.com/w/cpp/io/basic_stringstream/basic_stringstream)

### `w6` Module (supplied)

The tester module for the in-lab portion has been supplied. **Do not modify the existing code!**

When doing the workshop, you are encouraged to write your own tests, focusing on a single implemented feature at the time, until you get all functionality in place.

### Sample Output

When the program is started with the command (the file `cars.txt` is provided):“`w6.exe cars.txt“`the output should look like the one from the `sample_output.txt` file.

“`Command Line:————————–1: w6.exe2: cars.txt————————–

——————————–| Cars in the autoshop!        |——————————–|     Toyota | new    | 157.00 ||      Honda | broken | 145.00 ||   Chrysler | new    | 173.00 |——————————–“`

## *At-Home*

The *at-home* part of this workshop upgrades your *in-lab* solution to include one more module and update some existing modules:– `Racecar`– `Car` (to be updated)– `Autoshop` (to be updated)– `Utilities` (to be updated)– `w6` (to be updated)

### `Racecar` Module

Add a `Racecar` module to your project. A `Racecar` is a `Car` that for a short period of time can boost its top speed by a certain percent (this class should inherit `Car`).

***Private Data***

– `double m_booster`: the percentage (stored as a number between 0 an 1) by which this `Racecar` can boost its top speed.

***Public Members***

– `Racecar(std::istream&amp; in)`: calls the constructor from `Car` to build the subobject, and then it extracts the last field from the stream containing the booster bonus. The input format for a racecar is `TAG,MAKER,CONDITION,TOP_SPEED,BOOSTER`– `void display(std::ostream&amp; out) const`: calls `Car::display()` to print the information about the car, and adds `*` at the end. The format should be `| MAKER | CONDITION | TOP_SPEED |*`– `double topSpeed() const`: returns the top speed of the car, including any booster effects.

This class should not have access to the attributes of the parent class.

### `Car` Module

Update the `Car` module to handle invalid records, and generate exceptions when an invalid record is detected. This requires you to modify the custom constructor to detect the following situations:– the token representing the condition the car is empty (no characters or only blanks):– consider that the car is **new**– the token representing the condition of the car is a different character than `n`, `u`, or `b`:– generate an exception to inform the client that this record is invalid– the token representing the top speed is not a number:– generate an exception to inform the client that this record is invalid

No need to change anything else.

### `Utilities` module

Update the `createInstance` to build an instance of type `Racecar` if the first non-blank character extracted from the stream is `r` of `R`.

If the first non-blank character is not `c`, `C`, `r`, or `R` this function should generate an exception, informing the client that this record contains information about an unknown vehicle.

If there is no more information to be extracted from the stream, this function should return `nullptr`.

### `Autoshop` Module

Update this module to include two additional public functions.

***Public Members***

– a destructor. This function should iterate over the objects stored in the vector, and delete each one of them (note that the in-lab portion has a memory leak because the dynamically allocated vehicles were not deleted anywhere).

– `void select(T test, std::list&lt;const Vehicle*&gt;&amp; vehicles)`: a **template** function that iterates over the vehicles stored in the `m_vehicles`, and adds to the second parameter all vehicles for which the `test` is true. The first parameter (`test`) can be a lambda expression, a pointer to a function, or a functor that matches the prototype:“`bool func(const sdds::Vehicle*);“`

**Since this is a template function, it must be implemented in the header!** The class is not a template.

### `w6` Module (partially supplied)

This module has some missing parts. The missing parts are marked with `TODO`, describing what code you should add and where. **Do not modify the existing code, only add what is missing!**

### Sample Output

When the program is started with the command (the files are provided):“`w6.exe dataClean.txt dataMessy.txt“`the output should look like the one from the `sample_output.txt` file.

“`Command Line:————————–1: w6.exe2: dataClean.txt3: dataMessy.txt————————–

——————————–| Cars in the autoshop!        |——————————–|     Toyota | new    | 157.00 ||     Jaguar | used   | 295.20 |*|      Honda | broken | 145.00 ||     Porche | used   | 365.40 |*|   Chrysler | new    | 173.00 |——————————–

Invalid record!Invalid record!Unrecognized record type: [t]——————————–| Cars in the autoshop!        |——————————–|     Toyota | new    | 157.00 ||     Jaguar | used   | 295.20 |*|      Honda | broken | 145.00 ||     Porche | used   | 365.40 |*|   Chrysler | new    | 173.00 || Alfa Romeo | new    | 176.00 ||       Ford | new    | 162.00 ||   Red Bull | broken | 346.56 |*|    Ferrari | new    | 367.33 |*——————————–

——————————–|       Fast Vehicles          |——————————–|     Porche | used   | 365.40 |*|   Red Bull | broken | 346.56 |*|    Ferrari | new    | 367.33 |*——————————–

——————————–| Vehicles in need of repair   |——————————–|      Honda | broken | 145.00 ||   Red Bull | broken | 346.56 |*——————————–“`