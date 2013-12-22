---
layout: post
title: "java-bits #1: filedragdrop handler"
date: 2010-11-02 19:06
comments: true
---
drop a file from the explorer to a textbox and make the path the text of that textbox:

``` java
// imports omitted, dont know why...

	public class Foo extends TransferHandler {
		//.../*DRAG &amp;amp; DROP*/
		@Override
		public boolean importData(JComponent comp, Transferable t) {
			// Make sure we have the right starting points
			if (!(comp instanceof JTextField)) {
				return false;
			}
			if (!t.isDataFlavorSupported(DataFlavor.javaFileListFlavor)) {
				return false;
			}
			// Grab the tree, its model and the root node
			JTextField textField = (JTextField) comp;
			try {
				List data = (List) t.getTransferData(DataFlavor.javaFileListFlavor);
				for (Object object : data) {
					File f = (File) object;
					textField.setText(f.getAbsolutePath());
				}
				return true;
			} catch (UnsupportedFlavorException ufe) {
				System.err.println("Ack! we should not be here.\nBad Flavor.");
			} catch (IOException ioe) {
				System.out.println("Something failed during import:\n" + ioe);
			}
			return false;
		}

		@Override
		public boolean canImport(JComponent comp, DataFlavor[] transferFlavors) {
			if (comp instanceof JTextField) {
				for (DataFlavor transferFlavor : transferFlavors) {
					if (transferFlavor.equals(DataFlavor.javaFileListFlavor)) {
						return true;
					}
				}
				return false;
			}return false;
		}
	}
```