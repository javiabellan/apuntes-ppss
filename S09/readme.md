# Pruebas de aceptación 2

Lo malo de **Katalon** es que no soporta:

* Bucles
* Condiciones
* Gestión de errores
* Tests en la costrucción de proyecto

Por ello se puede usar **WebDriver** que usa **java** (entre otros)
para programar los tests de **pruebas emergentes funcionales**
sobre **aplicaciones web** en diferentes navegadores.

WebDriver usa un driver concreto según el navegador que estemos usando

```java
WebDriver driver = new FirefoxDriver();
driver.get("http://www.google.com");
```

Una página web está formada por elementos HTML, que se denominan **WebElements** en el contexto de WebDriver.

```java
WebElement searchBox = driver.findElement(By.name("q"));
```
Lacalizar:
* `By.name("username")`
* `By.id("loginForm")`
* `By.className("required")`
* `By.linkText("Register here")`
* `By.partialLinkText("here")`
* `By.xpath(????)`
* `By.cssSelector("input[value='Business']")`

Una vez localizado, podemos realizar acciones.

```java
searchBox.clear();
searchBox.sendKeys("Packt Publishing");
searchBox.submit();
```

Otras acciones comunes a todos los WenElements son:
* `click()`
* `getAttribute()`
* `getLocation()`
* `getText()`
* `isDisplayed()`
* `isEnabled()`
* `isSelected()`

Ejemplo de un test:

```java
@Test
public void signIn()
{
  WebDriver driver = new FirefoxDriver();
  driver.get("https://accounts.google.com");
  driver.manage().window().maximize();

  String expectedTitle = "Inicio de sesión - Cuentas de Google";
  String actualTitle   = driver.getTitle();
  Assert.assertEquals("Url incorrecta", expectedTitle, actualTitle);

  WebElement username = driver.findElement(By.id("Email"));
  username.clear();
  username.sendKeys("TestSelenium"); //tecleamos el usuario

  WebElement password = driver.findElement(By.id("Passwd"));
  password.clear();
  password.sendKeys("password123"); //tecleamos el password

  WebElement SignInButton = driver.findElement(By.id("signIn"));
  SignInButton.click();
  
  expectedTitle = "Inicio de sesión - Cuentas de Google";
  actualTitle   = driver.getTitle();
  Assert.assertEquals("Sign In Fallido", expectedTitle,actualTitle);

  driver.close(); //cerramos el navegador
}
```

Si no esperamos un ratico a que se carge la página, es probable que no se cargue lo que buscamos
(y se lanze un `NoSuchElementException`)

Así que podemos esperar de forma implicita: Esperar un tiempo sí o sí.

```java
WebDriver driver = new FirefoxDriver();
driver.manage().timeouts().implicitlyWait(10,TimeUnit.SECONDS);
driver.get("www.google.com");
```

O esperar hasta que aparezca un WebElement concreto  (explicito):

```java
//Create Wait using WebDriverWait.
//This will wait for 10 seconds for timeout before
//title is updated with search term
WebDriverWait wait = new WebDriverWait(driver, 10);
wait.until(ExpectedConditions.titleContains("selenium"));
```
