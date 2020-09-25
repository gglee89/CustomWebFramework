## **ABOUT THIS PROJECT**

Custom Web Framework

<br>

## **DESCRIPTION**

**Initial Architecture:**  
![image](https://user-images.githubusercontent.com/16644017/93847150-16e26b80-fce1-11ea-9b25-f6098e58cd6e.png)

**User Class:**  
![image](https://user-images.githubusercontent.com/16644017/93958289-23be9800-fd91-11ea-9389-4df74e512b9b.png)

**User functional description before injecting Composition:**  
![image](https://user-images.githubusercontent.com/16644017/93985408-01903e80-fdc0-11ea-8a12-8984ca52144f.png)

**Operation in Main Delegation:**  
![image](https://user-images.githubusercontent.com/16644017/93995249-be3bcd00-fdcb-11ea-80ee-02e46874d8a5.png)

<br><br>

## **DESIGN PATTERN**

<br>

## **User Class**

**Option #1:**  
Accept dependencies as second constructor argument.

```
...
export class User {
  constructor(
    private data: UserProps,
    private events: Eventing
  ) {}
  ...
}

new User({ id: 1 }, new Eventing());
```

> This pattern looks somewhat nasty/cluttered .

<br>  
  
**Option #2:**  
Only accept dependencies into constructor, and define a static class method to preconfigure User and assign properties afterwards. (i.e.: Similar to the MatchReader example)

The contructor is responsible to receive the "Composable" sub classes to aggregate into the main class.

```
...
export class User {
  static fromData(data: UserProps) {
    const user = new User(new Eventing());
    user.set(data);
    return user;
  }

  private data: UserProps;

  constructor(private events: Eventing) {}
  ...
}
```

> This pattern looks as an improvement over option #1, however we need to consider that.

> Using a static function for initialization may turn out to become a hurdle, repetitive and too much aglomeration in on point when considering scaling the main User class in future development iterations.

<br>

**Option #3:**  
Only accept properties into constructor.
Hard code dependencies as class properties.

```
...
export class User {
  events: Eventing = new Eventing();

  constructor(private data: UserProps) {}
  ...
}

const user = new User({});


user.events
```

> Pro: After instantiating a new object, events is already available for use without any need of extra configuration.

> Con: Lose some of the benefits of COMPOSITION. We can't swap out Eventing from class user. Making the class construction less flexible and modular.

<br>

## **Sync Class**

<br>

**Option #1:**
![image](https://user-images.githubusercontent.com/16644017/94100660-1f65ae00-fe69-11ea-8a84-8ed16b4afa30.png)

> Con: [BAD MODULARITY] Class Sync, in this case, would work exclusively with User having use to create a separate custom SYNC for each ENTITY in the Database.

> Pro: It works well enough for an application with a single ENTITY (e.g.: Users)

<br>

**Options #2:**
![image](https://user-images.githubusercontent.com/16644017/94101984-03afd700-fe6c-11ea-9703-6d3048b0924c.png)

> Con: Serializable and Deserializable interfaces receives an empty object making the design to LOSE ON TYPE SAFETY.

> Pro: [MODERATE MODULARITY] Mkaing use of INTERFACES, allows the design to be more generic and to accept a broader spectrum of income and outcome data flowing in the application.

<br>

**Options #3:**
![image](https://user-images.githubusercontent.com/16644017/94102235-a5372880-fe6c-11ea-95ba-e73d9d470935.png)

> Con: Higher complexity to implement the design in code. The use of Generics <T> adds up the level of difficulty to meddle

> Pro: [HIGH MODULARITY and INTEGRATION]

<br>

## **COMPOSITION w/ DELEGATION**

![image](https://user-images.githubusercontent.com/16644017/94219826-aaef4580-ff22-11ea-8a02-a1e7aa6669d5.png)

![image](https://user-images.githubusercontent.com/16644017/94219923-dbcf7a80-ff22-11ea-8ca1-7d680b0de921.png)

<br>

## **AUTHOR**

All contents are from **Stephen Grider**. A course author on Udemy.

I do not have any interests to show it as my work, but instead I mainly included in this documentation file for Reference and Documentation purposes so I have a better grasp how different moving parts are orchestrated to deliver this **Custom Web Framework**.

If this content infringes any copyright, I'll remove it immediately.
