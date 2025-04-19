(base) [bigdata@localhost Desktop]$ sudo yum install newt
(base) [bigdata@localhost Desktop]$ sudo apt install whiptail
(base) [bigdata@localhost Desktop]$ nano hdfs_gui.sh
-
-
-
-

(bash script file)
#!/bin/bash

# Show a menu using whiptail with new options
CHOICE=$(whiptail --title "HDFS Command Menu" \
--menu "Choose a command to run:" 15 60 6 \
"1" "List files in root HDFS directory (/)" \
"2" "List files in a specified HDFS directory (/path)" \
"3" "Upload File to HDFS" \
"4" "Download File from HDFS" \
"5" "Create Directory in HDFS" \
"6" "Remove File from HDFS" \
"7" "Exit the tool" \
3>&1 1>&2 2>&3)

# Check if user pressed OK or Cancel
if [ $? -eq 0 ]; then
    case $CHOICE in

        1)
 # List files in root HDFS directory
            OUTPUT=$(hdfs dfs -ls / 2>&1)
            ;;

        2)
            # List files in a specified HDFS directory
            DIR=$(whiptail --inputbox "Enter the HDFS directory path:" 10 60 "/$
            OUTPUT=$(hdfs dfs -ls "$DIR" 2>&1)
            ;;

        3)
            # Upload a file to HDFS
            LOCAL_FILE=$(whiptail --inputbox "Enter the local file path:" 10 60$
            HDFS_PATH=$(whiptail --inputbox "Enter the HDFS destination path:" $
            OUTPUT=$(hdfs dfs -put "$LOCAL_FILE" "$HDFS_PATH" 2>&1)
            ;;

        4)
            # Download a file from HDFS
            HDFS_FILE=$(whiptail --inputbox "Enter the HDFS file path:" 10 60 "$
            LOCAL_PATH=$(whiptail --inputbox "Enter the local path to save the $
            OUTPUT=$(hdfs dfs -get "$HDFS_FILE" "$LOCAL_PATH" 2>&1)
            ;;

        5)
            #Create a directory in HDFS
            DIR_PATH=$(whiptail --inputbox "Enter the HDFS directory path to cr$

            # Check if the parent directory exists
            PARENT_DIR=$(dirname "$DIR_PATH")
            if ! hdfs dfs -test -d "$PARENT_DIR"; then
                OUTPUT="Error: Parent directory $PARENT_DIR does not exist."
            else
                OUTPUT=$(hdfs dfs -mkdir "$DIR_PATH" 2>&1)
            fi
            ;;

        6)
            # Remove a file from HDFS
            FILE_PATH=$(whiptail --inputbox "Enter the HDFS file path to remove$
            OUTPUT=$(hdfs dfs -rm "$FILE_PATH" 2>&1)
            ;;

        7)
            # Exit script
            exit 0
            ;;

    esac

    # Show the command output in a textbox
    echo "$OUTPUT" > /tmp/hdfs_output.txt
    whiptail --title "Command Output" --textbox /tmp/hdfs_output.txt 25 80

else
    echo "You canceled the operation."
fi

-
-
-
-
-


(base) [bigdata@localhost Desktop]$ chmod +x hdfs_gui.sh
(base) [bigdata@localhost Desktop]$ ./hdfs_gui.sh

-
-







![Uploading WhatsApp Image 2025-04-19 at 11.09.13 PM.jpeg…]()


