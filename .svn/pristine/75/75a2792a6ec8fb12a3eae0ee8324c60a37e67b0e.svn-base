/*
 * TCSS 305
 * Assignment 4 - SnapSop
 */

package gui;

import filters.EdgeDetectFilter;
import filters.EdgeHighlightFilter;
import filters.Filter;
import filters.FlipHorizontalFilter;
import filters.FlipVerticalFilter;
import filters.GrayscaleFilter;
import filters.SharpenFilter;
import filters.SoftenFilter;
import image.PixelImage;
import java.awt.BorderLayout;
import java.awt.FlowLayout;
import java.awt.GridLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.File;
import java.io.IOException;
import javax.swing.ImageIcon;
import javax.swing.JButton;
import javax.swing.JFileChooser;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JPanel;


/**
 * The GUI for the SnapShop.
 * @author Yunhan Tu    
 * @version 2017/11/5
 */
public class SnapShopGUI {
   
    /**
     *  The main frame for GUI.
     */
    private final JFrame myFrame = new JFrame();
    
    /** 
     * File chooser. 
     */
    private final JFileChooser myChooser = new JFileChooser(".");
    
    /** 
     * Label for the image. 
     */
    private final JLabel myLabel = new JLabel(new ImageIcon());

    /** 
     * Panel for the filter buttons. 
     */
    private final JPanel mySevenPanel = new JPanel(new GridLayout(7, 1));
    
    /** 
     * Panel for the menu buttons. 
     */
    private final JPanel myThreePanel = new JPanel(new GridLayout(3, 1));
    
    /** 
     * Image display.
     */
    private PixelImage myImage;
    
    /** 
     * Array for filters.
     */
    private final Filter[] mySevenFilters = new Filter[] {
        new EdgeDetectFilter(), new EdgeHighlightFilter(),
        new FlipHorizontalFilter(), new FlipVerticalFilter(),
        new GrayscaleFilter(), new SharpenFilter(),
        new SoftenFilter()};
    
    /** 
     * Array for menu buttons. 
     * */
    private final JButton[] myThreeFilter = new JButton[] {
        new JButton("Open..."), new JButton("Save As..."),
        new JButton("Close Image")};  
    
    /**
     * Construct.
     */
    protected SnapShopGUI() {
        super();
    }
    
    /** 
     * create and add button to the panel.
     */
    private void createButtons() {
        for (int i = 0; i < mySevenFilters.length; i++) {
            mySevenPanel.add(filterButtonAndListen(mySevenFilters[i]));
        }

        myThreeFilter[0].addActionListener(new ActionListener() {
            public void actionPerformed(final ActionEvent theEvent) {
                final int result = myChooser.showOpenDialog(myFrame);
                if (result == JFileChooser.APPROVE_OPTION) {
                    try {
                        myImage = PixelImage.load(myChooser.getSelectedFile());
                        myLabel.setIcon(new ImageIcon(myImage));
                        buttonCondition(mySevenPanel, true); 
                        buttonCondition(myThreePanel, true); 
                        myFrame.pack(); 
                    } catch (final IOException e) {
                        JOptionPane.showMessageDialog(myFrame, 
                                                      "The selected file did "
                                                      + "not contain an image!", 
                                                      "Open Error!", 
                                                      JOptionPane.ERROR_MESSAGE);
                    }
                }
            }
        });
        myThreePanel.add(myThreeFilter[0]);
        
        myThreeFilter[1].addActionListener(new ActionListener() {
            public void actionPerformed(final ActionEvent theEvent) {
                saveHelper();
            }
        });
        myThreePanel.add(myThreeFilter[1]);
        
        myThreeFilter[2].addActionListener(new ActionListener() {
            public void actionPerformed(final ActionEvent theEvent) {   
                myLabel.setIcon(new ImageIcon());
                buttonCondition(mySevenPanel, false);
                myThreeFilter[1].setEnabled(false);
                myThreeFilter[2].setEnabled(false);
                myFrame.pack();
            }
        });
        
        myThreePanel.add(myThreeFilter[2]);
    }
    
    /**
     * To set up each Panel location and add to another panel.
     * @return mainPanel the panel that have everything
     */
    private JPanel addPanelup() {
        final JPanel mainPanel = new JPanel(new BorderLayout());
        final JPanel buttonPanel = new JPanel(new BorderLayout());
        final JPanel labelPanel = new JPanel(new FlowLayout());
        buttonPanel.add(mySevenPanel, BorderLayout.NORTH);
        buttonPanel.add(myThreePanel, BorderLayout.SOUTH);
        labelPanel.add(myLabel);  
        mainPanel.add(buttonPanel, BorderLayout.WEST);
        mainPanel.add(labelPanel, BorderLayout.CENTER);
        return mainPanel;
        
    }
    
    /**
     * Creates a filter button and listener.
     * @param theFilter the filter button.
     * @return button The filter button.
     */
    private JButton filterButtonAndListen(final Filter theFilter) {
        
        final JButton button = new JButton(theFilter.getDescription());
      
        button.addActionListener(new ActionListener() {

            public void actionPerformed(final ActionEvent theEvent) {
                theFilter.filter(myImage); 
                myLabel.setIcon(new ImageIcon(myImage)); 
            }
        });
        return button;
    }
    
    
    /**
     * Helper method for checks file exist or not.
     */
    private void saveHelper() {
        final int result = myChooser.showSaveDialog(myFrame);
        final File file = myChooser.getSelectedFile();
        if (result == JFileChooser.APPROVE_OPTION && file.exists()) {
            final int replace = JOptionPane.showConfirmDialog(myFrame, 
                                                                 "Do you want to replace it?", 
                                                                 "Existing File ", 
                                                            JOptionPane.YES_NO_CANCEL_OPTION);
            if (replace == JOptionPane.YES_OPTION) {
                try { 
                    myImage.save(file);
                } catch (final IOException e) {
                    JOptionPane.showMessageDialog(myFrame,  
                                                  "Can not save it!", 
                                                  "Save Error!",
                                                  JOptionPane.ERROR_MESSAGE);
                }
            } else if (replace == JOptionPane.NO_OPTION) {
                saveHelper();
            }
        } else if (result == JFileChooser.APPROVE_OPTION && !file.exists()) { 
            try { 
                myImage.save(file);
            } catch (final IOException e) {
                JOptionPane.showMessageDialog(myFrame,  
                                              "File can not be save!", 
                                              "Save Error.",
                                              JOptionPane.ERROR_MESSAGE);
            }
        }
    }
    

    
    /**
     * Method for enabling/disabling buttons.
     * @param thePanel The panel 
     * @param theBool  enable or disable.
     */
    private void buttonCondition(final JPanel thePanel, final boolean theBool) {
        for (int i = 0; i < thePanel.getComponentCount(); i++) {
            if (theBool) { 
                thePanel.getComponent(i).setEnabled(true);
            } else { 
                thePanel.getComponent(i).setEnabled(false);
            }
        }
    }
    
  
    
    /**
     * set the the main frame.
     */
    public void start() {
        createButtons();       
        myFrame.add(addPanelup(), BorderLayout.WEST);
        myFrame.setTitle("TCSS 305 SnapShop");
        myFrame.setResizable(true);
        myFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        myFrame.pack();
        myFrame.setLocationRelativeTo(null);
        buttonCondition(mySevenPanel, false);
        myThreeFilter[1].setEnabled(false);
        myThreeFilter[2].setEnabled(false);
        myFrame.setVisible(true);
    }

}