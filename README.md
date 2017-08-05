# LinkListApplication_with_GUI
schoolActivity


/**
 * Write a description of class Link_List_Apps here.
 * 
 * @author (your name) 
 * @version (a version number or a date)
 */
import java.util.Scanner;
public class Link_List_Apps
{
    public static SingleLinkedList<Integer> sll = new SingleLinkedList<Integer>();
    static Scanner console = new Scanner(System.in);
    public static void main()
    {
        char option = ' ';
        User userInfo = new User("archievalcamposano","password101"); //variable that handles the valid username and password
        
        do
        {
            try
            {
                System.out.print("\nLink List Apps:"
                                +"\n\t[C] Console Interface"
                                +"\n\t[G] Graphical User Interface"
                                +"\n\t[E] Exit"
                                +"\n\nEnter Option: ");
                option = console.next().charAt(0);
                switch(option)
                {
                    //invokes the simpleMenuLinkList
                    case 'C': case 'c':
                        SimpleMenuLinkList sample = new SimpleMenuLinkList();
                        sample.main(userInfo);
                        break;
                    //invokes the Graphical User Interface Login
                    case 'G': case 'g':
                        Login.invokeLogin(userInfo);    //invokes the Login
                        break;
                    //terminate the program/options
                    case 'E': case 'e':
                        System.out.println("Exit Program!!!");
                        System.exit(0);
                        break;
                    //default result if input is invalid
                    default:
                        System.out.println("Invalid Input!!!");
                        break;
                }
            }
            catch(Exception eRef)
            {
                System.out.println("Invalid Input!!!");
            }
        }while(true);
    }//end of method main
}//end of class Link_List_Apps


import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
public class Login extends JFrame
{
    private JLabel headingL1,headingL2,usernameL,passwordL;
    private JTextField usernameTF,passwordTF;
    private JButton loginB,exitB;
    private ButtonHandler pbHandler;
    private User sampleUser;
    public Login(User validUserInfo)
    {
        sampleUser = validUserInfo;
        setVisible(true);
        headingL1 = new JLabel("Login ",SwingConstants.RIGHT);
        headingL2 = new JLabel("Member",SwingConstants.LEFT);
        usernameL = new JLabel("Username",SwingConstants.CENTER);
        passwordL = new JLabel("Password",SwingConstants.CENTER);
        usernameTF = new JTextField();
        passwordTF = new JTextField();
        loginB = new JButton("Login");
        exitB = new JButton("Exit");
        pbHandler = new ButtonHandler();
        loginB.addActionListener(pbHandler);
        exitB.addActionListener(pbHandler);
        Container pane  = getContentPane();
        setSize(400,300);
        pane.setLayout(new GridLayout(4,2));
        pane.add(headingL1);
        pane.add(headingL2);
        pane.add(usernameL);
        pane.add(usernameTF);
        pane.add(passwordL);
        pane.add(passwordTF);
        pane.add(loginB);
        pane.add(exitB);    
        setDefaultCloseOperation(EXIT_ON_CLOSE);
    }// end of the Modified Option constructor
    //class ButtonHandler and the action to be performed of a certain event(user clicks the corresponding button)
    private class ButtonHandler implements ActionListener
    {
        public void actionPerformed(ActionEvent e)
        {
            if(e.getActionCommand().equals("Exit"))
            {   
                JOption.message("Exit Program","Exit",'E');
                setVisible(false);  
                //System.exit(0);
            }
            else if(e.getActionCommand().equals("Login"))
            {
                //prompts the user if the inputs are valid or not and executes if login is success
                if(sampleUser.getUsername().equals(usernameTF.getText().trim())&& sampleUser.getPassword().equals(passwordTF.getText().trim()))
                {
                    JOption.message("Login Successfully","Login",'I');
                    setVisible(false);
                    LinkListMenuGUI.invokeLinkListMenuGUI();
                }
                else
                {
                    if(!sampleUser.getUsername().equals(usernameTF.getText().trim()) && sampleUser.getPassword().equals(passwordTF.getText().trim()))
                    {
                        JOption.message("Invalid username","Login",'W');
                    }
                    else if(!sampleUser.getPassword().equals(passwordTF.getText().trim())&&sampleUser.getUsername().equals(usernameTF.getText().trim()))
                    {
                        JOption.message("Invalid password","Login",'W');
                    }
                    else
                    {
                        JOption.message("Invalid username and password","Login",'E');
                    }
                }
            }
        }
    }
    //method to invoke the Login Menu
    public static void invokeLogin(User validUserInfo)
    {
        Login sample = new Login(validUserInfo);
    }
}

import java.util.*;
/*****************************************************************************************************
 * Write a program that prompts user to Login first, once the user has successly login
 * an option will appear where the user can use the following Link List applications
 * Menu for LINK LIST APPS:
 *      [A] Add
 *      [B] Search
 *      [C] Delete
 *      [D] Insert
 *      [E] Exit
 * 
 * Add   : prompts the user to input a value and stores or add it in the list(tail)
 * Search: prompts the user to input a value to be search and checks if it is in the list otherwise
 *         prompts the user that it is not found in the list
 * Delete: prompts the user to input a value to be deleted within the list if true otherwise 
 *         prompts the user that the input is not found in the list
 * Insert: prompts the user to input a value to be insert*(somewhere)* within the list
 * Exit: terminates the program.
 * 
 * @author (your name) 
 * @version (a version number or a date)
 */


public class SimpleMenuLinkList
{
   static Scanner console = new Scanner(System.in);
   /********************************************************************************
    * method to accepts username and password and                                  *
    * returns true if the username and password is correct otherwise false         *
    * Pre:  userInfo is the valid username and password as parameter of the method *
    *       accepts username and password from the user input                      *
    *       and compare it to the valid user's info                                *
    * Post: if username and password is correct returns true otherwise false       *
    ********************************************************************************/
   public static boolean loginMember(User userInfo)
   {
       System.out.print("\fLOGIN MEMEBER\n\tEnter Username: ");
       String user = console.next().trim(); //assign user's username input to the user
       System.out.print("\tEnter Password: ");    
       String pass = console.next().trim(); //assign user's password input to the pass
       
       //determine if the user's input are valid and returns the correspoding boolean value 
       return (userInfo.getUsername().equals(user) && userInfo.getPassword().equals(pass))? true: false;
   }//end of method loginMember
   /******************************************************************
    * method to prompts the user to input the desired link list apps *
    * options are add,search,delete,insert,exit                      *
    * Pre: prompts user and accepts the user's input                 *
    * Post: determine what operation to be excuted base on           *
    * the user's desired the link list methods                       *
    ******************************************************************/
   public static void linkListMenu(/*SingleLinkedList<T> sLinkedList*/) throws Exception
   {
       char option = ' ';
       do{
           //prompts user to input the following valid options:
           System.out.println("\nLink List Apps"
                          +"\n\n\t[A] Add"
                          +"\n\t[B] Search"
                          +"\n\t[C] Delete"
                          +"\n\t[D] Insert"
                          +"\n\t[E] Exit");
           //prompts user to input option
           System.out.print("\n\nEnter Option: ");
           option = console.next().charAt(0);
           switch(option)
           {
               //add Node to the Link list
               case 'A': case 'a':
                    //sLinkedList.add();
                    break;
               //search node to the link list
               case 'B': case 'b':
                    //sLinkedList.search();
                    break;
               //delete node in the link list
               case 'C': case 'c':
                    //sLinkedList.delete();
                    break;
               //insert a node within the link list
               case 'D': case 'd':
                    //sLinkedList.insert();
                    break;
               //exit program
               case 'E': case 'e':
                    //prompts the user that the program is terminated
                    JOption.message("Exit Program","Exit",'E');
                    break;
               //default 
               default:
                    //prompts the user that the input is invalid
                    JOption.message("Invalid Input", "Options",'W');
                    break;
           }//end of switch
        }while(option!= 'E' && option != 'e');
   }//end of method linkListMenu
   /******************************************************************************************
    * Summary:                                                                               *
    *       username and password is initialized to have a valid user's info,                *
    *       invokes the method loginMethod to prompt user to input username and password     *
    *       and determines whether the inputs are valid or not,if both username and password *
    *       are valid then prompts the user that has successfully login and execute the      *
    *       linkListMenu method. Link list method prompts the user to input the following    *
    *       valid Options to manipulates the Link list. Add,Search,Delete,Insert and Exit    *
    *       after the user inputs the desired options it executes it correspoding            *
    *       methods to be executed.                                                          *
    * Pre: username and password is intialized as valid Login pass                           *
    * Post: logiMethod is invoked if user info is valid returns true; otherwise false        *
    *       if loginMethod is true invokes the linkListMenu                                  *
    *       if loginMethod is false prompt user that invalid user info                       *   
    ******************************************************************************************/
   public static void main(User validUserInfo)throws Exception
   {
       User userInfo = validUserInfo;
       //variable that handles the single link list
       //SingleLinkedList<Integer> sLinkedList = new SingleLinkedList();    
       do
       {
           try
           {
               //invokes the method Login with its 
               //corresponding parameter(userInfo[valid username and password])
               if(loginMember(userInfo))
               {
                   //prompts user that has successfully login
                   JOption.message("Login Successfully","Login",'I');
                   //invokes the method linkListMenu
                   linkListMenu(/*sLinkedList*/);
                   break;
               }
               else
               {
                   JOption.message("Invalid username or password\nTry Again","Login",'E');
               }//end of if statement
           }
           catch(Exception ePrompt)
           {
               System.out.println("Invalid Input!!!");
           }//end of try catch
        }while(true);
   }//end of method main
}//end of class SimpleMenuList


import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
/**
 * Summary:
 *      Class LinkListMenuGUI inherates the class JFrame 
 *      it has 3 variables: 
 *          headingMainL(Header Label)
 *          Array of JButton optionsB(correspoding ButtonsLabels)
 *          pbHandler;
 *      Methods:
 *       constructor with parameters:
 *          public LinkListMenuGUI(desired Header,number of buttons,button labels);
 *       other methods:
 *          public invokeLinkListMenuGUI();
 */
public class LinkListMenuGUI extends JFrame
{
    private JLabel headingMainL;       //handles the variable for the heading of the Menu
    private JButton[] optionsB;        //handles the variable for the buttons 
    private ButtonHandler pbHandler;   
    // default constructor of the class with its corresponding parameter
    // String Heading parameter is to set the String value of the Jlabel headingMainL
    // int numOfbuttons is to set the number of buttons of this class 
    // An array of String is to handle the names or label of the buttons 
    public LinkListMenuGUI(String Heading,int numOfButtons,String[] nameOfButton)
    {
        setVisible(true);
            //instantiating the headingMainL to a JLabel type and set is Name on the 
            //corresponding String Heading of the parameter and set its position to center
            headingMainL = new JLabel(Heading,SwingConstants.CENTER);
            //instantiate the array of optionsB to JButton and its array size 
            //which is the number of buttons from the parameter
            optionsB = new JButton[numOfButtons];
            //instansting the elements of the array optionsB to Jbutton type and
            //its corresponding name or label of button passed from the parameter (the array of String)
            for(int x = 0;x<numOfButtons;x++)
                optionsB[x] = new JButton(nameOfButton[x]);
            //instantiating the phhandler to an object Buttonhandler    
            pbHandler = new ButtonHandler();
           //
            for(int x = 0;x<numOfButtons;x++)
                optionsB[x].addActionListener(pbHandler);
            Container pane  = getContentPane();
            setSize(400,500);
            pane.setLayout(new GridLayout(numOfButtons+1,1));
            
            pane.add(headingMainL);
            for(int x = 0;x<numOfButtons;x++)
                pane.add(optionsB[x]);
                
        setDefaultCloseOperation(EXIT_ON_CLOSE);
    }// end of the Modified Option constructor
    //class ButtonHandler and the action to be performed of a certain event(user clicks the corresponding button)
    private class ButtonHandler implements ActionListener
    {
        public void actionPerformed(ActionEvent e)
        {
            try
            {
                if(e.getActionCommand().equals("Exit"))
                {   
                    setVisible(false);
                   //System.exit(0);
                }
                //adds a node to the Link list
                else if(e.getActionCommand().equals("Add"))
                {
                    //Link_List_Apps.sll.add(JOption.promptUser("Enter A number to be added: "));
                }
                //Search a value within the Link List 
                else if(e.getActionCommand().equals("Search"))
                {
                     //Link_List_Apps.sll.search(JOption.promptUser("Enter A number to be search: "));
                }
                //Delete node within the Link list
                else if(e.getActionCommand().equals("Delete"))
                {
                     //Link_List_Apps.sll.deleteAfter(JOption.promptUser("Enter a number to be deleted: "));
                }
                //Insert a node within the Link List
                else if(e.getActionCommand().equals("Insert"))
                {
                    //sll.insert();
                }
            }
            catch(Exception eRef)
            {
                JOption.message("Invalid Input","Input",'W');
            }
        }//end of class ButtonHandler
    }
    //method to invokes the Link List menu
    public static void invokeLinkListMenuGUI()
    {
        String[] menuButtonsLabels = {"Add","Search","Delete","Insert","Exit"};
        LinkListMenuGUI sample = new LinkListMenuGUI("Link List Apps",menuButtonsLabels.length,menuButtonsLabels);
    }//end of method invokeLinkListMenuGUI
}//end of class LinkListMenuGUI()


/**
 * Write a description of class OtherMethods here.
 * 
 * @author (your name) 
 * @version (a version number or a date)
 */
import javax.swing.*;
public class JOption
{
    //method for Prompt Messages in Input
    public static int promptUser(String promtMessage)
    {
        return Integer.parseInt(JOptionPane.showInputDialog(promtMessage));
    }//end of promptUser method
    //Method for Messaging the user with its corresponding JOption type 
    public static void message(String message,String title,char typeOfMessageDialog)
    {
        switch(typeOfMessageDialog)
        {
            case 'P':
                JOptionPane.showMessageDialog(null,message,title,JOptionPane.PLAIN_MESSAGE);
                break;
            case 'E':
                JOptionPane.showMessageDialog(null,message,title,JOptionPane.ERROR_MESSAGE);
                break;
            case 'I':
                JOptionPane.showMessageDialog(null,message,title,JOptionPane.INFORMATION_MESSAGE);
                break;
            case 'W':
                JOptionPane.showMessageDialog(null,message,title,JOptionPane.WARNING_MESSAGE);
                break;
        }
    }//end of JOptionMessage method
}


/**
 * Class for the User
 * User composes of String varaibles, 
 * A username and a password
 * 
 * methods of User class:
 *   default constructor: 
 *      public User(); 
 *   constructor with parameter: 
 *      public User(args);
 *   other methods:
 *      public setUser(args);
 *      public setUsername(args);
 *      public setPassword(args);
 *      public getUsername();
 *      public getPassword();
 *      
 * @author (Archie Val Camposano) 
 * @version (Aug. ,2017)
 */
public class User
{
  private String username;  //handles the value of the username
  private String password;  //handles the value of the password
  /*
   * default constructor assigns the username and password to null
   * post-conditions: username = null; password = null;
   */
  public User()
  {
      username = "";    //set the value of the username to null
      password = "";    //set the value of the password to null
  }
  /*
   * constructor with parameter that set the username and password
   */
  //method to sets the value of the username
  public User(String usernameInput, String passwordInput)
  {
      setUser(usernameInput,passwordInput);
  }
  //method that sets the user with its corresponding values within the parameter
  public void setUser(String usernameInput,String passwordInput)
  {
      username = usernameInput.trim();  //sets the value of the username
      password = passwordInput.trim();  //sets the value of the password
  }
  //method to set the value of username
  public User setUsername(String usernameInput)
  {
    username = usernameInput.trim();
    return this;
  }
  //method to set the value of the password
  public User setPassword(String passwordInput)
  {
    password = passwordInput.trim();
    return this;
  }
  //method that returns the value of the username
  public String getUsername()
  {
      return username;
  }
  //method that returns the value of the password
  public String getPassword()
  {
      return password;
  }
}//end of class User


/**
 * Write a description of class Class_node here.
 * 
 * @author (your name) 
 * @version (a version number or a date)
 */
public class Node<T> 
{
   protected T info;
   protected Node<T> link;
   
   
   public Node()
   {
       info = null;
       link = null;
   }
   
   //Methods for the Nodes
   //method that returns the value of the info of the node
   public T getValue()
   {
       return info;
   }
   //method that sets the value of the info of the node
   public void setValue(T value)
   {
       this.info = value;
   }
   //method that returns the value of the link
   public Node<T> getNextRef()
   {
       return link;
   }
   //method that sets the value of the link of the node 
   public void setNextRef(Node<T> next)
   {
       this.link = next;
   }
   //override the method compare
   public int compareTo(T arg)
   {
       if(arg == this.info)
            return 0;
       else
            return 1;
   }
}


/**
 * Write a description of class SingleLinkedListImpl here.
 * 
 * @author (your name) 
 * @version (a version number or a date)
 */
  
public class SingleLinkedList<T>
{
   private Node<T> head;
   private Node<T> tail;
   
   public void add(T element){
       Node<T> nd = new Node<T>();
       nd.setValue(element);
       JOption.message("Adding: " + element,"Add",'I');
       //check if the list is sempty
       if(head == null)
       {
           //since there is only one element, both head and 
           //tail points to the same object
           head = nd;
           tail = nd;
       }
       else
       {
           //set current tail next link to new node
           tail.setNextRef(nd);
           //set tail as newly created node
           tail = nd;
       }
    }
    
   public void addAfter(T element, T after)
    {
        Node<T> temp = head;
        Node<T> refNode = null;
        JOption.message("Traversing to all nodes..","Add After",'I');
        //Traversing till given element
        while(true)
        {
            if(temp == null)
                break;
            if(temp.compareTo(after) == 0)
            {
                //found the target node, add after this node
                refNode = temp;
                break;
            }
            temp = temp.getNextRef();
        }
        if(refNode != null)
        {
            //add element after the target mode
            Node<T> nd = new Node<T>();
            nd.setValue(element);
            nd.setNextRef(temp.getNextRef());
            if(temp == tail)
                tail = nd;
            temp.setNextRef(nd);
        }
        else
            JOption.message("unable to find the given element...","Add After",'W');
        
   }
   
   public void deleteFront()
   {
       if(head == null)
          JOption.message("Underflow...","Delete",'W');
       Node<T> temp = head;
       head = temp.getNextRef();
       if(head == null)
          tail = null;
       JOption.message("Deleted: "+ temp.getValue(),"Delete",'I');
   }
   public void deleteAfter(T after)
   {
    Node<T> temp = head;
    Node<T> refNode = null;
    JOption.message("Traversing to all nodes..","Delete After",'I');
    while(true)
    {
        if (temp == null)
            break;
        if(temp.compareTo(after) == 0)
        {
            refNode = temp;
            break;
        }
        temp = temp.getNextRef();
    }
    if(refNode != null)
    {
        temp = refNode.getNextRef();
        refNode.setNextRef(temp.getNextRef());
        if(refNode.getNextRef() == null)
            tail = refNode;
        JOption.message("Deleted: "+ temp.getValue(),"Delete After",'I');
    }
    else
        JOption.message("Unable to find the given element...","Delete After",'W');
    
   }
   
   public void traverse()
   {
       Node<T> temp = head;
       while(true){
           if(temp == null)
                break;
           System.out.print(temp.getValue() + " ");
           temp = temp.getNextRef();
        }
        System.out.println();
   }
   
}
