package homeinventory;
import javax.swing.*;
import javax.swing.filechooser.*;
import java.awt.*;
import java.awt.event.*;
import java.beans.*;
import com.toedter.calendar.*;
import java.awt.geom.*;
import java.io.*;
import java.util.*;
import java.text.*;
import java.awt.print.*;
public class HomeInventory extends JFrame
{
// Toolbar
JToolBar inventoryToolBar = new JToolBar();
JButton newButton = new JButton(new ImageIcon("new.gif"));
JButton deleteButton = new JButton(new ImageIcon("delete.gif"));
JButton saveButton = new JButton(new ImageIcon("save.gif"));
JButton previousButton = new JButton(new ImageIcon("previous.gif"));
JButton nextButton = new JButton(new ImageIcon("next.gif"));
JButton printButton = new JButton(new ImageIcon("print.gif"));
JButton exitButton = new JButton();

// Frame
JLabel itemLabel = new JLabel();
JTextField itemTextField = new JTextField();
JLabel locationLabel = new JLabel();
JComboBox locationComboBox = new JComboBox();
JCheckBox markedCheckBox = new JCheckBox();
JLabel serialLabel = new JLabel();
JTextField serialTextField = new JTextField();
JLabel priceLabel = new JLabel();
JTextField priceTextField = new JTextField();
JLabel dateLabel = new JLabel();
JDateChooser dateDateChooser = new JDateChooser();
JLabel storeLabel = new JLabel();
JTextField storeTextField = new JTextField();
JLabel noteLabel = new JLabel();
JTextField noteTextField = new JTextField();
JLabel photoLabel = new JLabel();
static JTextArea photoTextArea = new JTextArea();
JButton photoButton = new JButton();
JPanel searchPanel = new JPanel();
JButton[] searchButton = new JButton[26];
PhotoPanel photoPanel = new PhotoPanel();
static final int maximumEntries = 300;
static int numberEntries;
static InventoryItem[] myInventory = new InventoryItem[maximumEntries];
int currentEntry;
static final int entriesPerPage = 2;
static int lastPage;
public static void main(String args[])
{
// create frame
new HomeInventory().show();
}
public HomeInventory()

{
// frame constructor
setTitle("Home Inventory Manager");
setResizable(false);
setDefaultCloseOperation(JFrame.DO_NOTHING_ON_CLOSE);
addWindowListener(new WindowAdapter()
{
public void windowClosing(WindowEvent evt)
{
exitForm(evt);
}
});
getContentPane().setLayout(new GridBagLayout());
GridBagConstraints gridConstraints;
inventoryToolBar.setFloatable(false);
inventoryToolBar.setBackground(Color.BLUE);
inventoryToolBar.setOrientation(SwingConstants.VERTICAL);
gridConstraints = new GridBagConstraints();
gridConstraints.gridx = 0;
gridConstraints.gridy = 0;
gridConstraints.gridheight = 8;
gridConstraints.fill = GridBagConstraints.VERTICAL;
getContentPane().add(inventoryToolBar, gridConstraints);
inventoryToolBar.addSeparator();
Dimension bSize = new Dimension(70, 50);
newButton.setText("New");
sizeButton(newButton, bSize);
newButton.setToolTipText("Add New Item");
newButton.setHorizontalTextPosition(SwingConstants.CENTER);
newButton.setVerticalTextPosition(SwingConstants.BOTTOM);
newButton.setFocusable(false);
inventoryToolBar.add(newButton);

newButton.addActionListener(new ActionListener()
{
public void actionPerformed(ActionEvent e)
{
newButtonActionPerformed(e);
}
});
deleteButton.setText("Delete");
sizeButton(deleteButton, bSize);
deleteButton.setToolTipText("Delete Current Item");
deleteButton.setHorizontalTextPosition(SwingConstants.CENTER);
deleteButton.setVerticalTextPosition(SwingConstants.BOTTOM);
deleteButton.setFocusable(false);
inventoryToolBar.add(deleteButton);
deleteButton.addActionListener(new ActionListener()
{
public void actionPerformed(ActionEvent e)
{
deleteButtonActionPerformed(e);
}
});
saveButton.setText("Save");
sizeButton(saveButton, bSize);
saveButton.setToolTipText("Save Current Item");
saveButton.setHorizontalTextPosition(SwingConstants.CENTER);
saveButton.setVerticalTextPosition(SwingConstants.BOTTOM);
saveButton.setFocusable(false);
inventoryToolBar.add(saveButton);
saveButton.addActionListener(new ActionListener()
{
public void actionPerformed(ActionEvent e)
{

saveButtonActionPerformed(e);
}
});
inventoryToolBar.addSeparator();
previousButton.setText("Previous");
sizeButton(previousButton, bSize);
previousButton.setToolTipText("Display Previous Item");
previousButton.setHorizontalTextPosition(SwingConstants.CENTER);
previousButton.setVerticalTextPosition(SwingConstants.BOTTOM);
previousButton.setFocusable(false);
inventoryToolBar.add(previousButton);
previousButton.addActionListener(new ActionListener()
{
public void actionPerformed(ActionEvent e)
{
previousButtonActionPerformed(e);
}
});
nextButton.setText("Next");
sizeButton(nextButton, bSize);
nextButton.setToolTipText("Display Next Item");
nextButton.setHorizontalTextPosition(SwingConstants.CENTER);
nextButton.setVerticalTextPosition(SwingConstants.BOTTOM);
nextButton.setFocusable(false);
inventoryToolBar.add(nextButton);
nextButton.addActionListener(new ActionListener()
{
public void actionPerformed(ActionEvent e)
{
nextButtonActionPerformed(e);
}

});
inventoryToolBar.addSeparator();
printButton.setText("Print");
sizeButton(printButton, bSize);
printButton.setToolTipText("Print Inventory List");
printButton.setHorizontalTextPosition(SwingConstants.CENTER);
printButton.setVerticalTextPosition(SwingConstants.BOTTOM);
printButton.setFocusable(false);
inventoryToolBar.add(printButton);
printButton.addActionListener(new ActionListener()
{
public void actionPerformed(ActionEvent e)
{
printButtonActionPerformed(e);
}
});
exitButton.setText("Exit");
sizeButton(exitButton, bSize);
exitButton.setToolTipText("Exit Program");
exitButton.setFocusable(false);
inventoryToolBar.add(exitButton);
exitButton.addActionListener(new ActionListener()
{
public void actionPerformed(ActionEvent e)
{
exitButtonActionPerformed(e);
}
});
itemLabel.setText("Inventory Item");
gridConstraints = new GridBagConstraints();
gridConstraints.gridx = 1;
gridConstraints.gridy = 0;

gridConstraints.insets = new Insets(10, 10, 0, 10);
gridConstraints.anchor = GridBagConstraints.EAST;
getContentPane().add(itemLabel, gridConstraints);
itemTextField.setPreferredSize(new Dimension(400, 25));
gridConstraints = new GridBagConstraints();
gridConstraints.gridx = 2;
gridConstraints.gridy = 0;
gridConstraints.gridwidth = 5;
gridConstraints.insets = new Insets(10, 0, 0, 10);
gridConstraints.anchor = GridBagConstraints.WEST;
getContentPane().add(itemTextField, gridConstraints);
itemTextField.addActionListener(new ActionListener ()
{
public void actionPerformed(ActionEvent e)
{
itemTextFieldActionPerformed(e);
}
});
locationLabel.setText("Location");
gridConstraints = new GridBagConstraints();
gridConstraints.gridx = 1;
gridConstraints.gridy = 1;
gridConstraints.insets = new Insets(10, 10, 0, 10);
gridConstraints.anchor = GridBagConstraints.EAST;
getContentPane().add(locationLabel, gridConstraints);
locationComboBox.setPreferredSize(new Dimension(270, 25));
locationComboBox.setFont(new Font("Arial", Font.PL…
