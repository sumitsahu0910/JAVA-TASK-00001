import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Timer;
import java.util.TimerTask;

public class AlarmClockApp extends JFrame {
    private JTextField timeField;
    private JButton setAlarmButton;
    private Timer timer;

    public AlarmClockApp() {
        setTitle("Alarm Clock");
        setSize(300, 150);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setLayout(new FlowLayout());

        timeField = new JTextField(10);
        setAlarmButton = new JButton("Set Alarm");

        setAlarmButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                setAlarm();
            }
        });

        add(new JLabel("Set alarm time (HH:mm:ss): "));
        add(timeField);
        add(setAlarmButton);
    }

    private void setAlarm() {
        try {
            SimpleDateFormat dateFormat = new SimpleDateFormat("HH:mm:ss");
            Date alarmTime = dateFormat.parse(timeField.getText());

            long delay = alarmTime.getTime() - System.currentTimeMillis();

            if (delay < 0) {
                JOptionPane.showMessageDialog(this, "Please enter a future time for the alarm!");
                return;
            }

            timer = new Timer();
            timer.schedule(new TimerTask() {
                @Override
                public void run() {
                    JOptionPane.showMessageDialog(AlarmClockApp.this, "Time's up! Alarm activated.");
                    timer.cancel();
                }
            }, delay);
            
            JOptionPane.showMessageDialog(this, "Alarm set for " + timeField.getText());
        } catch (ParseException ex) {
            JOptionPane.showMessageDialog(this, "Invalid time format! Please use HH:mm:ss.");
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                new AlarmClockApp().setVisible(true);
            }
        });
    }
}
