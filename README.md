[![Open in Codespaces](https://classroom.github.com/assets/launch-codespace-7f7980b617ed060a017424585567c406b6ee15c891e84e1186181d67ecf80aa0.svg)](https://classroom.github.com/open-in-codespaces?assignment_repo_id=11267765)
# UI Lab 3

В ході практичної роботи було виконано **наступне завдання** - [Робота 3: GUI з Swing](https://github.com/ppc-ntu-khpi/GUI-Lab1-Starter/blob/master/Lab%203%20-%20SWING/Lab%203.md) на оцінку 5. 

## Код доданого класу: 

```java
import com.mybank.data.DataSource;
import com.mybank.domain.Bank;
import com.mybank.domain.CheckingAccount;
import com.mybank.domain.Customer;
import java.awt.BorderLayout;
import java.awt.Dimension;
import java.awt.GridLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.File;
import java.io.IOException;
import java.net.URLDecoder;
import javax.swing.JButton;
import javax.swing.JComboBox;
import javax.swing.JEditorPane;
import javax.swing.JFrame;
import javax.swing.JPanel;

/**
 *
 * @author Angel
 */
public class SWINGDemo {

    private final JEditorPane log;
    private final JEditorPane reportLog;
    private final JButton show;
    private final JButton report;
    private final JComboBox clients;

    public SWINGDemo() {
        log = new JEditorPane("text/html", "");
        reportLog = new JEditorPane("text/html", "");

        log.setPreferredSize(new Dimension(250, 450));

        show = new JButton("Show");
        report = new JButton("Report");
        clients = new JComboBox();
        for (int i=0; i<Bank.getNumberOfCustomers();i++)
        {
            clients.addItem(Bank.getCustomer(i).getLastName()+", "+Bank.getCustomer(i).getFirstName());
        }

    }

    private void launchFrame() {
        JFrame frame = new JFrame("MyBank clients");
        frame.setLayout(new BorderLayout());
        JPanel cpane = new JPanel();
        cpane.setLayout(new GridLayout(1, 2));

        cpane.add(clients);
        cpane.add(show);
        cpane.add(report);
        frame.add(cpane, BorderLayout.NORTH);
        frame.add(log, BorderLayout.CENTER);
        frame.add(reportLog, BorderLayout.AFTER_LAST_LINE);


        show.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                Customer current = Bank.getCustomer(clients.getSelectedIndex());
                String accType = current.getAccount(0)instanceof CheckingAccount?"Checking":"Savings";
                String custInfo="<br>&nbsp;<b><span style=\"font-size:2em;\">"+current.getLastName()+", "+
                        current.getFirstName()+"</span><br><hr>"+
                        "&nbsp;<b>Acc Type: </b>"+accType+
                        "<br>&nbsp;<b>Balance: <span style=\"color:red;\">$"+current.getAccount(0).getBalance()+"</span></b>";
                log.setText(custInfo);
            }
        });

        report.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String reportInfo = "&nbsp;<span style=\"font-size:2em;\"><b>Customers Report</b></span>";
                for (int i = 0; i < Bank.getNumberOfCustomers(); i++) {
                    Customer customer = Bank.getCustomer(i);
                    reportInfo += "<hr><b>&nbsp;<span style=\"font-size:1em;\">"+customer.getLastName() + ", " + customer.getFirstName() + "</span></b><br>";
                    for (int j = 0; j < customer.getNumberOfAccounts(); j++) {
                        String accType = customer.getAccount(j) instanceof CheckingAccount ? "Checking" : "Savings";
                        reportInfo += "&nbsp;<b>Account Type: </b>" + accType +
                                "<br>&nbsp;<b>Balance: <span style=\"color:green;\">$" + customer.getAccount(j).getBalance() + "<br></span></b><br>";
                    }
                }
                reportLog.setText(reportInfo);
            }
        });

        frame.pack();
        frame.setLocationRelativeTo(null);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setResizable(false);
        frame.setVisible(true);
    }

    public static void main(String[] args) throws IOException {


        File currentClassFile = new File(URLDecoder.decode(SWINGDemo.class
                .getProtectionDomain()
                .getCodeSource()
                .getLocation()
                .getPath(), "UTF-8"));
        String classFileDirectory = currentClassFile.getParent();
        DataSource data = new DataSource(classFileDirectory+"\\test.dat");
        data.loadData();

        SWINGDemo demo = new SWINGDemo();
        demo.launchFrame();
    }

}
```
## Результат:

![image](https://github.com/ppc-ntu-khpi/35-gui-1-angelxdem/assets/113301385/29d692cd-c801-45af-a499-3cdfa365d972)
![image](https://github.com/ppc-ntu-khpi/35-gui-1-angelxdem/assets/113301385/48cdc526-835a-43ea-b370-6da111a34f83)
![image](https://github.com/ppc-ntu-khpi/35-gui-1-angelxdem/assets/113301385/26f877d0-0ebe-4f4d-8cd4-a0f5b0d4ffaf)



