# Install ApE on Linux  

by Glen Ernstrom  

last update: 2024-02-17

Here is a guide to get ApE up and running on Linux. I doubt I covered all 
possible scenarios, but I have tested ApE on the latest versions of Pop\_OS! (an
Ubuntu variant), Fedora, Arch, and Debian, in the GNOME desktop environment, 
and it works great! Wayne's README that comes with his .zip ApE download may be sufficient for you, but here is a bit more detail that I found worked for me.

I have ApE running on NixOS but I still need to tweak the configuration file to 
get the ApE's desktop launcher to appear in the GNOME application launcher and 
for ApE to automatically find the 'Accessory Files' file. In Nix, I can run 
ApE with ./ApE.tcl with ApE saved to a nix store, but when it launches it asks 
for the 'Accessory Files' folder which I can direct to and it launches and runs 
perfectly. I'll provide a derivative.nix file in a separate file once I figure 
it out.
  
## Basic setup
  
  1. [Download ApE]
  (https://jorgensen.biology.utah.edu/wayned/ape/Download/Linux/ApE_linux_current.zip)
  
  2. Extract the file in a convenient location (e.g. you can create a folder
     called "Applications" in your home directory ~/Applications) For this 
     example, I extracted the downloaded ApE_linux_current.zip file to 
     ~/Applications
     
  3. Install tk8.6 with root privieleges for your flavor of Linux
  
     - For Debian/Ubuntu based Linux: $ sudo apt install tk8.6
     - For Arch/Endeavor: $ sudo pacman -S tk
     - For Fedora/Red Hat:$ sudo dnf install tk
     - Working on a derivation for NixOS. It can launch but I don't have it fully optimized yet. If you know of one, please share! 
       
     It is possible the latest version of tk is already installed.
     
  4. Confirm ApE can launch from the command line. For example:
     /usr/bin/wish8.6 ~/Applications/ApE_linux_current/ApE Linux/ApE.tcl

     You should see the first launch splash screen and then the ApE editor 
     window.
     
  5. You can make ApE.tcl executable: (e.g. with chmod +x ApE.tcl) then,
     in the same directory as ApE.tcl, launch from the command line with
     ./ApE.tcl. If this does not work go to next step and try again.
     
  6. To launch from the command line with ./ApE.tcl, you may need to make a 
     symbolic link to 'wish': sudo ln -s /usr/bin/wish8.6 /usr/bin/wish.
     
## Cosmetics  
  
  1. From a file browser "e.g. GNOME Files" Right click on ApE.tcl
  
  2. Select Properties 
  
  3. Click on the generic icon in the ApE properties windown
  
  3. Navigate to Accessory Files in the ApE Linux folder
  
  4. Open Icons and images folder.
  
  5. Select ApE\_icon.png
  
## Add ApE to the Application Launcher

Not essential, but recommend editing ApE.desktop to latest Free Desktop 
standard.

  1. Open ApE.desktop (in 'Accessory Files' or create a new ApE.desktop file in 
     a test editor.
  
  2. Edit your file to resemble this:

         [Desktop Entry]  
         Version=1.5  
         Type=Application  
         Name=ApE  
         Comment=A plasmid Editor  
         GenericName=DNA Editor  
         Icon=your-absolute-path-to-the-ApE_icon.png-file 
         Exec="your-absolute-path-to-the-ApE.tcl" 
         MimeType=application/ape  
         Categories=Science;Biology  

Example: 

  Icon=/home/ernstrom/Apps/ApE_linux_current/ApE Linux/Accessory Files/ \
  Icons and images/ApE_icon.png  

  Include the spaces in file names in the Icon and Exec keys (the text after 
  each equal sign). Check for typos. Follow the same for the Exec key but 
  enclose in double quotes. 

  3. Copy the newly saved ApE.desktop file .local/share/applications: 

         cp ApE.desktop ~/.local/share/applications
         
  4. Make ApE.desktop executable (this may not be absolutely necessary):

         chmod +x ~/.local/share/applications/ApE.desktop
         
## Assign MIME types to ApE

So that when you click on a .ape file it will launch ApE.

  1. Create a MIME .xml (e.g. ape-mime-type.xml) file for ApE that looks 
     like this:
  
    <?xml version="1.0" encoding="UTF-8"?>
      <mime-info xmlns="http://www.freedesktop.org/standards/shared-mime-info">
          <mime-type type="application/ape">
          <comment>DNA plasmid files</comment>
            <glob pattern="*.ape"/>
            <glob pattern="*.str"/>
            <glob pattern="*.dna"/>
            <glob pattern="*.seq"/>
            <glob pattern="*.gb"/>
          </mime-type>
      </mime-info>

  2. Save this file to /usr/share/mime/packages (as root or with sudo).
  
  3. Update the mime database:  

          sudo update-mime-database /usr/share/mime
          
  4. Update the applicaion launcher icons: 

         sudo update-desktop-database ~/.local/share/applications/
         
  5. Usually that does it! You should see ApE and its beautiful icon in your 
     application launcher. You may need to restart or logout and then login to     see the icon appear in your application launcher.        
