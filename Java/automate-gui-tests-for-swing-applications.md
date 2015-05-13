# Automate GUI tests for Swing applications

## Transition from unit tests to acceptance tests

Over the last two years, I spent some time developing a GUI application using Java Swing. The application was small, consisting of several classes in the MVC (Model-View-Controller) model, but was moderately complicated, having many external components with which to communicate. To avoid total confusion, my team applied extreme programming (XP) methods, which emphasize testing, as much as we could to the project. But we encountered technical problems testing the view part: how would we perform unit tests and automate acceptance tests?

Test-driven development, with unit tests, has always been easy and straightforward using JUnit driven by Ant. We started having difficulties with our project when we began developing the view component and testing it. We were unaware of a good technical option for unit-testing GUI components.

On the other hand, an acceptance test is a less formalized process than a unit test in terms of tools for automating it. Sometimes, human testers perform acceptance tests to check the application's behavior as a whole. But, for a GUI application, how the application responds to human operations is equally important as its underlying functioning. We like to run acceptance tests frequently during development. Therefore, automated and comprehensive acceptance tests proved important.

We started with simple unit tests that solved some of our technical problems. To automate our acceptance tests, we applied the same techniques used in the unit tests to the application's acceptance tests (or story tests). We were then able to write tests easily and run them frequently, which is essential for XP.

### Sample application

Let me explain what my team learned using a simple application, shown in Figure 1.
                    <figure class="center"><img
                            src="automate-gui-tests-for-swing-applications_files/jw-1115-swing1-100156585-orig.gif"
                            height="73" width="297">
                        <figcaption>Figure 1. Sample Swing application</figcaption>
                    </figure>

The application has a text field. When a string is typed, it adds <code>?</code> to the end. When the Doit! button is clicked, a dialog box displays the text—see Figure 2.
                    <figure class="center"><img
                            src="automate-gui-tests-for-swing-applications_files/jw-1115-swing2-100156586-orig.gif"
                            height="98" width="298">
                        <figcaption>Figure 2. Modal dialog box when the button is clicked</figcaption>
                    </figure>

The application also has a menu for changing the text color (Figure 3).
                    <figure class="center"><img
                            src="automate-gui-tests-for-swing-applications_files/jw-1115-swing3-100156587-orig.gif"
                            height="95" width="302">
                        <figcaption>Figure 3. Menu items to change the input field text color</figcaption>
                    </figure>

Of course, according to XP's "test-first" rule, application code should not exist prior to the test code. But for this article's purpose, we start from this completed code and focus on the test code.</p>

### Access Swing components from test code

First, we define the problem. Our aim is to automate acceptance tests, namely to test the code of our application, and type its strings and click its buttons. Our application uses Swing components, and JUnit is our testing framework. In generic terms, our aims can be written as "Using JUnit, test code can access an application's Swing component" and "Test code can change and retrieve a component's states." The latter is easy once we get an instance of the component.

There are many ways to access Swing components:

1. Application code has `getXxx()` methods to return each component of interest.
2. Test code invokes events on a screen, mimicking a human operator. Events are typically mouse moves/clicks and key typing.
3. Test code traverses the component tree and finds a component of a specific signature (class, location, order, text contents, etc.).

When choosing a method, two factors are important. One is simplicity and the other is independence between application and test code.

The first method requires writing numerous lines of application code, especially when more and more components are involved in the test. If you change what will be tested, application code will also change. We better avoid this dependency.</p>

The second choice does not modify the target code at all. Application code is written as if no test exists. The standard Java library has a <code>java.awt.Robot</code> class for this purpose. But test code becomes awfully complicated. Just to click a button, we must specify the coordinate to which a pointer must move to perform the click. Every time the geometrical arrangement changes, the entire test code needs new parameters.</p>

My team chose the third method and used <em>name</em> as a component's signature. Naming a component is fairly simple—just a line with a `setName()` method. The modification in the application code is small. And the test code doesn't require complicated intelligence to find appropriate components selected by its class or location. The method is not fragile to changes in the target code. "Name" is receptive to changes unlike a component's absolute location, ordered number, or label string. What happens if the order of the test input field for first name and family name are exchanged? When specified with a "name," no change is necessary.

Now let's test the code. The following snippet is the relevant part of the unit-test code. Simple enough, isn't it?

```
(test/FooTest.java)
...
14   public class FooTest extends TestCase {
15   
16      static Foo foo;
...
26      public void testTypeIn() throws Exception {
27         String testString = "message1";
28   
29         assertNotNull(foo);  // Instantiated?
30   
31         JTextField input = (JTextField)TestUtils.getChildNamed(foo, "input");
32         assertNotNull(input); // Component found?
33   
34         input.setText(testString);
35         input.postActionEvent();  // Type in a test message + ENTER
36   
37         assertEquals(testString + "?", input.getText());
38      }
...

```

Component traversal is encapsulated into a utility class, `TestUtils`. The `TestUtils.getChildNamed()` static method looks for a Swing component named `input`, starting from the application's `JFrame` class (or from any `Component` object in the Swing tree structure):

```
(test/TestUtils.java)
...
14      public static Component getChildNamed(Component parent, String name) {
15   
16         // Debug line
17         //System.out.println("Class: " + parent.getClass() +
18         //    " Name: " + parent.getName());
19   
20         if (name.equals(parent.getName())) { return parent; }
21   
22         if (parent instanceof Container) {
23            Component[] children = ((Container)parent).getComponents();
26   
27            for (int i = 0; i < children.length; ++i) {
28               Component child = getChildNamed(children[i], name);
29               if (child != null) { return child; }
30            }
31         }
32         
33         return null;
34      }
...
```

Only a single line should be added to the application code:

```
(Foo.java)
...
66         // Test input field, add "?" to the text when ENTER is hit.
67         // inputField is a instance variable
68         inputField = new JTextField(20);
69         inputField.addActionListener(new ActionListener() {
70            public void actionPerformed(ActionEvent event) {
71               inputField.setText(inputField.getText() + "?");
72            }
73         });
74         inputField.setName("input");
75         getContentPane().add(inputField);
...
```

### A trick for menu items

For menu items, the application code and test code are similar. But a small change is necessary in the `TestUtils.getChildNamed()` method to access a menu item, which is not realized until it is dropped down:

```
(test/TestUtils.java)
...
14      public static Component getChildNamed(Component parent, String name) {
...
22         if (parent instanceof Container) {
23            Component[] children = (parent instanceof JMenu) ?
24                  ((JMenu)parent).getMenuComponents() :
25                  ((Container)parent).getComponents();
...
```

### Modal dialog box

The hardest problem is the modal dialog box. When we create a dialog box with the `JOptionPane.showXxx()` method, two obstacles prevent a straightforward solution:

* Thread of execution doesn't return until the dialog box closes, because it is "modal"
* There is no way to "name" a component in a dialog box because dialog boxes are transient, created at the time of execution

The first problem is solved with the `SwingUtilities.invokeLater()` method.

There are several ways to solve the second problem. Writing our own dialog box class derived from the `JDialog` class is one solution. But we don't want to give up the simplicity of the `JOptionPane.showXxx()` methods. Assuming dialog boxes have a simple and predictable structure, we choose to use the component's class name and appearance order pair as a signature to find the component. Then we write another traversing method slightly modified from the `getChildNamed()` method:
                        
```
<span
                        > <code>(test<span
                        >/TestUtils.<span
                        >java)
...
12      static<span
                        > int counter<span
                        >;
...
36      public<span
                        > static Component<span
                        > getChildIndexed(
37            Component<span
                        > parent, String<span
                        > klass, int<span
                        > index) <span
                        >{
<strong>38         counter =<span
                        > 0;
</strong>39   
40         // Step in only owned windows and ignore its components in JFrame<span
                        >
41         if<span
                        > (parent instanceof<span
                        > Window)<span
                        > {
<strong>42            Component<span
                        >[] children =<span
                        > ((Window)<span
                        >parent).<span
                        >getOwnedWindows();
</strong>43   
44            for<span
                        > (int<span
                        > i = <span
                        >0; i <span
                        >&lt; children.<span
                        >length; ++<span
                        >i) <span
                        >{
45               <span
                        >// Take only active windows
46               if<span
                        > (children<span
                        >[i]<span
                        > instanceof <span
                        >Window &amp;&amp;<span
                        >
47                     !((<span
                        >Window)children<span
                        >[i]).isActive<span
                        >()) {<span
                        > continue;<span
                        > }
48   
49               Component<span
                        > child =<span
                        > getChildIndexedInternal(
50                     children[<span
                        >i], klass,<span
                        > index);
51               if<span
                        > (child !=<span
                        > null)<span
                        > { <span
                        >return child;<span
                        > }
52            }
53         }
54   
55         return<span
                        > null;
56      }
57   
58      private<span
                        > static Component<span
                        > getChildIndexedInternal(
59            Component<span
                        > parent, String<span
                        > klass, int<span
                        > index) <span
                        >{
60   
61         // Debug line<span
                        >
62         //System.out.println("Class: " + parent.getClass() +<span
                        >
63         //    " Name: " + parent.getName());<span
                        >
64   
<strong>65         if<span
                        > (parent<span
                        >.getClass().<span
                        >toString().endsWith<span
                        >(klass))<span
                        > {
66            if<span
                        > (counter <span
                        >== index)<span
                        > { <span
                        >return parent;<span
                        > }
67            ++counter<span
                        >;
68         }
</strong>69   
70         if<span
                        > (parent instanceof<span
                        > Container)<span
                        > {
71            Component<span
                        >[] children =<span
                        > (parent instanceof<span
                        > JMenu)<span
                        > ?
72                  ((<span
                        >JMenu)parent<span
                        >).getMenuComponents()<span
                        > :
73                  ((<span
                        >Container)parent<span
                        >).getComponents();<span
                        >
74   
75            for<span
                        > (int<span
                        > i = <span
                        >0; i <span
                        >&lt; children.<span
                        >length; ++<span
                        >i) <span
                        >{
76               Component<span
                        > child =<span
                        > getChildIndexedInternal(
77                     children[<span
                        >i], klass,<span
                        > index);
78               if<span
                        > (child !=<span
                        > null)<span
                        > { <span
                        >return child;<span
                        > }
79            }
80         }
81         
82         return<span
                        > null;
83      }
...
</code>
</pre>
                    <p>The test code is pretty straightforward:</p><pre style="" class="prettyprint prettyprinted"><span
                        > <code>(test<span
                        >/FooTest.java<span
                        >)
...
50      public<span
                        > void testPopUp<span
                        >() throws<span
                        > Exception <span
                        >{
51         final<span
                        > JButton popup <span
                        >= (JButton<span
                        >)TestUtils.<span
                        >getChildNamed(foo<span
                        >, "popup");<span
                        >
52         assertNotNull(<span
                        >popup);
53   
<strong>54         SwingUtilities<span
                        >.invokeLater(<span
                        >new Runnable<span
                        >() {
55            public<span
                        > void run()<span
                        > {
56               popup.<span
                        >doClick();
57            }
58         });
</strong>59   
60         JButton ok <span
                        >= null<span
                        >;
61   
62         // The dialog box will show up shortly<span
                        >
63         for<span
                        > (int<span
                        > i = <span
                        >0; ok <span
                        >== null<span
                        >; ++<span
                        >i) <span
                        >{
64            Thread.<span
                        >sleep(200);<span
                        >
65            ok =<span
                        > (JButton)<span
                        >TestUtils.<span
                        >getChildIndexed(foo<span
                        >, "JButton"<span
                        >, 0<span
                        >);
66            assertTrue(<span
                        >i &lt; <span
                        >10);
67         }
68         assertEquals(<span
                        >
69               UIManager<span
                        >.getString(<span
                        >"OptionPane.okButtonText"),<span
                        > ok.getText<span
                        >());
70   
71         ok.doClick<span
                        >();
72      }
...
</code>
</pre>


Please remember that an event resulting from the `invokeLater()` method does not execute immediately. We must wait for the button to be clicked and a dialog box to display. Also, we must make a dialog box owned by a window (application frame, in this case) and start traversing from that window. In the application code, we need to add only one line to name a button "popup."

### From unit tests to acceptance tests

So far, we have tested components in a Swing application for unit tests only. But, if you concatenate the contents of these unit tests in a particular order, you get an acceptance test for a single story. Here is an example.

```
(test/FooTest.java)
...
74      public void testStory() throws Exception {
75         // Type a string, change the color and popup
76   
77         String testString = "message2";
78   
79         JTextField    input = (JTextField)TestUtils.getChildNamed(foo, "input");
80         JMenuItem     red   = (JMenuItem)TestUtils.getChildNamed(foo, "red");
81         JMenuItem     blue  = (JMenuItem)TestUtils.getChildNamed(foo, "blue");
82         final JButton popup = (JButton)TestUtils.getChildNamed(foo, "popup");
83   
84         input.setText(testString);
85         input.postActionEvent();
86   
87         red.doClick();
88         assertEquals(testString + "?", input.getText());
89         assertEquals(Color.red, input.getForeground());
90   
91         blue.doClick();
92         assertEquals(testString + "?", input.getText());
93         assertEquals(Color.blue, input.getForeground());
94   
95         SwingUtilities.invokeLater(new Runnable() {
96            public void run() {
97               popup.doClick();
98            }
99         });
100 
101        JButton ok = null;
102        JTextArea message = null;
103  
104        // The dialog box will show up shortly
105        for (int i = 0; ok == null || message == null; ++i) {
106           Thread.sleep(200);
107           ok = (JButton)TestUtils.getChildIndexed(foo, "JButton", 0);
108           message = (JTextArea)TestUtils.getChildIndexed(foo, "JTextArea", 0);
109           assertTrue(i < 10);
110        }
111        assertEquals(
112              UIManager.getString("OptionPane.okButtonText"), ok.getText());
113        assertEquals(testString + "? ... done.", message.getText());
114  
115        ok.doClick();
116     }
...
```
