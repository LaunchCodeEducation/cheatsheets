Thymeleaf cheat sheet
=====================

This is a cheat sheet to summarize all the main Thymeleaf features and how to use them to kickstart you with Thymeleaf.

## What is Thymeleaf?


Thymeleaf is an engine that builds dynamic pages from templates that are written in XHTML with the help of some special attributes, so it is a **template engine**.

A template engine is an engine that parses XHTML/HTML pages that contain special tags or attributes or syntax that is bound to variables at the server, and resolves them into their values, then parses the page according to those values and builds a normal HTML page.

Thymeleaf is an **in-memory** template engine, so it does all of it's processing in memory, it builds a DOM that maps to the HTML of the page in-memory and when values change the parsed pages are changed, also it's caching is an in-memory caching system.

Thymeleaf is a template engine that relays mostly on **attributes** instead of tags like what JSP would do, this makes it testable in the browser directly without requiring a server.

Those attributes are then translated and processed by Thymeleaf into normal HTML.

## How it works

```html
<p th:text="'Thymeleaf will display this'">text</p>
```

Here Thymeleaf will process the text inside the `th:text` attribute, and replace the contents of the `<p>` tag with it.

We can use **variable expressions** syntax `${...}` to insert data passed in from a controller.

```html
<p th:text="${message}">text</p>
```

Thymeleaf works by replacing the contents of the tags that it's attributes are defined on. Another example is:

```html
<tr th:each="prod : ${prods}">
	<td th:text="${prod.name}">Onions</td>
	<td th:text="${prod.price}">2.41</td>
<tr>
```

Here Thymeleaf will repeat the `<tr>` with the list of products, this is defined by the attribute `th:each`, it will also remove the dummy content in both the `<td>` tags, and replace them with the content that is evaluated from `th:text="${prod.name}"` and `th:text="${prod.price}"`.

## Attributes

Thymeleaf is an attribute based template engine, it processes attributes and their values to build it's DOM tree.

* `th:text`: this attribute is responsible for displaying text that is evaluated from the expression inside it, it will process the expression and then display the text **html-encoded**,
Example:

```html
<p th:text="${home.welcome}">Welcome to our grocery store!</p>
```

* `th:utext`: Similar to previous attribute but this one display text **unescaped** for more information check [using_texts](http://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#using-texts)
* `th:attr` : Takes an HTML attribute and sets it's value dynamically, example: '
`<input type="submit" value="Subscribe me!" th:attr="value=${subscribe.submit}"/>`
The `value` attribute will be set to the value of `${subscribe.submit}` after processing, replacing the supplied `value="Subscribe me!"`
* `th:value`,`th:action`,`th:href, th:onclick`...etc: Those attributes can be used as a shorthand of the `th:attr` syntax as equally equivalent to it, so the attribute `th:action` is equal to `th:attr="action="`
* `th:attrappend`: This will not replace the attribute value, but will only append the value to it, example: `th:attrappend="class=${' ' + cssStyle}"`, for more information check [setting_attribute_values](http://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#setting-attribute-values)
* `th:each`: This is the iteration attribute, it is analogous to Java's for-each loop: `for(Object o : list)`, but its syntax is

	```html
	<tr th:each="prod,iterStat : ${prods}" th:class="${iterStat.odd}? 'odd'">
		<td th:text="${prod.name}">Onions</td>
		<td th:text="${prod.price}">2.41</td>
		<td th:text="${prod.inStock}? ${true} : ${false}">yes</td>
	</tr>
	```

	The `th:each="prod,iterStat : ${prods}"` is equivalent to `for(Product prod : prods)` and the `iterStat` is the status variable of the iteration, it contains information about current iteration like its number,index,total count ...etc.

	The iteration object `prod` can then be accessed in the context of the tag `<th>`, meaning it will only exist within the tag that it's been defined in, for more information check [iteration](http://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#iteration)

	When iterating over a map, the components of each item can be accessed using `.key` and `.value`:

	```html
	<tr th:each="mapItem : ${map}">
		<td th:text="${mapItem.key}">Key</td>
		<td th:text="${mapItem.value}">Value</td>
	</tr>
	```

* `th:if`: Evaluates the conditions specified in the attribute and if they are true, the tag is displayed, if not they are not displayed, example : `th:if="${user.admin}"`
* `th:unless`: Is the opposite of `th:if`, it will display the tag if the value is false, so `th:unless="${user.admin}"` is equal to `th:if="${!(user.admin)}"`
* `th:switch` and `th:case`: Those attributes are used to create a swtich statement, `th:switch` will hold the variable to switch on, and `th:case` will evaluate the case statements for this variable, example

```html
<div th:switch="${user.role}">
	<p th:case="'admin'">User is an administrator</p>
	<p th:case="${roles.manager}">User is a manager</p>
	<p th:case="*">User is some other thing</p>
</div>
```

For more information check [conditional_evaluation](http://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#conditional-evaluation)

## Expressions

Thymeleaf works based on many expressions, Thymeleaf has different expression syntax other than the traditional `${variablename.propertyname}` syntax, namely:

* `${message.in.proprties.file}` similar to the **i18n** resolver in **JSF**, this expressions will look for the value provided in the localization properties files provided to the application.
Example: `<p th:text="${brand.name}">Brand Name</p>`, when using spring it will use the `MessageSource` of spring
* `${variable}`: This is the variables expression, if your expression should evaluate to a variable or you have a variable in your `model` as an attribute, you must use this expression to access it, other expressions are used for different purposes and may not functional correctly with variables, example:
`<span th:text="${today}">13 february 2011</span>`

* Thymeleaf provides some predefined variables that can be accessed using the `${#variableName}` syntax and they are:

1. `#ctx` : the context object.
2. `#vars`: the context variables .
3. `#locale` : the context locale.
4. `#httpServletRequest` : (only in Web Contexts ) the         				`HttpServletRequest` object.
5. `#httpSession`: The session object of current session
6. `#dates` : utility methods for `java.util.Date` objects : formatting , component extraction, etc.
7. `#calendars` : analogous to #dates , but for `java.util.Calendar` objects .
8. `#numbers` : utility methods for formatting numeric objects .
9. `#strings` : utility methods for String objects : contains , startsWith, prepending /appending , etc.
10. `#objects` : utility methods for objects in general.
11. `#bools` : utility methods for boolean evaluation.
12. `#arrays` : utility methods for arrays .
13. `#lists` : utility methods for lists .
14. `#sets` : utility methods for sets .

Example:

```html
<span th:text="${#locale.country}">
```

and

```html
<span th:text="${#calendars.format(today,'dd MMMM yyyy')}">13 May 2011</span>
```

* `*{property}`: This is used the same way as the `${variable}` but works on selected objects, i.e. objects which are set using `th:object` attribute, for example

```html
<div th:object="${session.user}">
	<p>Name: <span th:text="*{firstName}">Sebastian</span>.</p>
	<p>Surname: <span th:text="*{lastName}">Pepper</span>.</p>
	<p>Nationality: <span th:text="*{nationality}">Saturn</span>.</p>
</div>
```

This will access properties on `${session.user}` object directly using the `*{...}` syntax, like for `*{firstName}`, this is equal to using `${session.user.firstName}`

*Note*: The `th:object` is defined only in the context of the tag it's declared on, meaning it's not available outside the context of that tag.

* `@{/link/path}`: This will create a link to the path specified relative to the deployment context, so if the application is deployed at context **my-app**, then the generated path will be **/my-app/link/path**.
To include parameters, use `@{/link/path(param=value)}`. This will generate **/link/path?param=value**
For Path variables use: `@{/link/{myPathVariable}/path(myPathVariable=${variable})}`
which will replace the **{myPathVariable}** with the value from **${variable}**

*  Literals: You can also write some normal literals instead of any expressions,
	* "'the literal string'": You can write normal strings between two **''**  single quotes
	*  "3 + 2": Normal numeric expressions
	* "false","true" and "null": are evaluated to normal `false`,`true` and `null` expressions
	* "singleWordToken": tokens with single words do not need single quotes and can be written as is.
* `${#fields}`:  Spring MVC adds another predefined variable which is `#fields`, it refers to `spring-form`  fields and their validation errors, mainly used for error validation
* `${@beanName.method()}`: Also spring specific bean method call expression, this will call a method on a spring bean called `beanName`, it will look for the bean in the current spring context
