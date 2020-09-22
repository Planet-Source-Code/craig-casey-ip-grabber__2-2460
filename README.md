<div align="center">

## IP Grabber


</div>

### Description

Accepts connections and takes the IP of the request
 
### More Info
 
You have to connect to it any way you want... whether it's a web browser or a self-written app that uses the Socket class


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Craig Casey](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/craig-casey.md)
**Level**          |Beginner
**User Rating**    |3.4 (17 globes from 5 users)
**Compatibility**  |Java \(JDK 1\.2\)
**Category**       |[Internet/ Browsers/ HTML](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/internet-browsers-html__2-68.md)
**World**          |[Java](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/java.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/craig-casey-ip-grabber__2-2460/archive/master.zip)

### API Declarations

This is a simple server app that gets the IPs or incoming requests...


### Source Code

```
// Sorry about the enormous pile of copyright
// stuff above. I didn't put it there... please
// ignore it.
//
// By: Craig Casey -> craig_c_11@hotmail.com
// This code, unlike Ian's non-legally-bound
// one-line entries that somehow manage to win
// contests, is PUBLIC DOMAIN.
// This means that ANYONE can use it without
// being harassed by me or anyone else.
// And NO, you do not need to have this comment
// block... in fact, you're encouraged to remove
// it from your code.
import java.net.*;
import java.io.*;
import java.awt.*;
import java.awt.event.*;
import javax.swing.*;
class StaticGrabberData {
	static JTextArea IPListArea = new JTextArea(0, 0);
}
class DaemonGrabber extends Thread {
	DaemonGrabber () {
		setDaemon(true);
		start();
	}
	public void run ()
	{
		try {
			while (true) {
				ServerSocket s = new ServerSocket(8080);
				try {
					new GrabberWorker(s.accept());
				} catch (Throwable t) {
					JOptionPane.showMessageDialog(null, "The server fuX0R3d up!");
				} finally {
					try {
						s.close();
					} catch (IOException e) {
						JOptionPane.showMessageDialog(null, "Could n0t c1053 th3 s3rv3r s0ck3t!");
					}
				}
			}
		} catch (IOException e) {
			JOptionPane.showMessageDialog(null, "This wh0l3 th4ng is fuX0R3d up!");
		}
	}
}
class GrabberWorker extends Thread {
	Socket s;
	GrabberWorker ()
	{
		// This is only here for... um... no reason.
	}
	GrabberWorker (Socket acceptedConnection)
	{
		setDaemon(false);
		s = acceptedConnection;
		start();
	}
	public void run ()
	{
		try {
			StaticGrabberData.IPListArea.append((s.getInetAddress()).getHostAddress()+"\n");
		} catch (Throwable t) {
			JOptionPane.showMessageDialog(null, "There was a problem r34ding the s0cke1.");
		} finally {
			try {
				s.close();
			} catch (IOException e) {
				JOptionPane.showMessageDialog(null, "Could n0t c1053 4 s0ck3t!");
			}
		}
	}
}
public class IPGrabber {
	IPGrabber ()
	{
		new DaemonGrabber();
		final JFrame jFr = new JFrame("IP Grabber");
		final JFileChooser jFC = new JFileChooser();
		jFr.getContentPane().setLayout(new GridLayout(1, 1));
		JPanel jPane = new JPanel();
		jPane.setLayout(new GridLayout(0, 1));
		JButton jbClear = new JButton("Clear");
		JButton jbSave = new JButton("Save");
		JButton jbOpen = new JButton("Open");
		jPane.add(jbClear);
		jPane.add(jbSave);
		jPane.add(jbOpen);
		JSplitPane jpSplit = new JSplitPane(JSplitPane.HORIZONTAL_SPLIT, new JScrollPane(StaticGrabberData.IPListArea), jPane);
		jpSplit.setDividerLocation(300);
		jFr.getContentPane().add(jpSplit);
		jFr.pack();
		jFr.setSize(400, 100);
		jbClear.addActionListener(
			new ActionListener () {
				public void actionPerformed(ActionEvent e)
				{
					StaticGrabberData.IPListArea.replaceRange("", 0, StaticGrabberData.IPListArea.getText().length());
				}
			}
		);
		jbSave.addActionListener(
			new ActionListener () {
				public void actionPerformed(ActionEvent e)
				{
					try {
						int isApproved = jFC.showSaveDialog(jFr);
						if (isApproved == JFileChooser.APPROVE_OPTION) {
							File f = jFC.getSelectedFile();
							PrintWriter pw = new PrintWriter(new BufferedWriter(new FileWriter(f)), true);
							pw.print(StaticGrabberData.IPListArea.getText());
							pw.close();
						}
					} catch (Throwable t) {
						JOptionPane.showMessageDialog(null, "C0u1d n0t s4v3 fi13!");
					}
				}
			}
		);
		jbOpen.addActionListener(
			new ActionListener () {
				public void actionPerformed(ActionEvent e)
				{
					try {
						int isApproved = jFC.showOpenDialog(jFr);
						if (isApproved == JFileChooser.APPROVE_OPTION) {
							File f = jFC.getSelectedFile();
							BufferedReader br = new BufferedReader(new FileReader(f));
							StaticGrabberData.IPListArea.replaceRange("", 0, StaticGrabberData.IPListArea.getText().length());
							int i;
							String fConts = new String("");
							while ((i = br.read()) != -1) {
								fConts += (char)i;
							}
							br.close();
							StaticGrabberData.IPListArea.append(fConts);
						}
					} catch (Throwable t) {
						JOptionPane.showMessageDialog(null, "C0u1d n0t s4v3 fi13!");
					}
				}
			}
		);
		jFr.addWindowListener(
			new WindowAdapter () {
				public void windowClosing(WindowEvent e)
				{
					System.exit(0);
				}
			}
		);
		jFr.setVisible(true);
	}
	public static void main (String[] argv)
	{
		new IPGrabber();
	}
}
```

