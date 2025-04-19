
# HDFS Command-Line GUI with Whiptail

This is a simple shell script tool that provides a graphical menu interface (using `whiptail`) to execute basic HDFS (Hadoop Distributed File System) commands. It helps users interact with the HDFS system through a user-friendly interface instead of remembering long terminal commands.

---

```bash
# For RHEL/CentOS systems:
sudo yum install newt

sudo yum install whiptail
```

---

## First

Create the script file:

```bash
nano hdfs_gui.sh
```

Then We Pasted the following code into `hdfs_gui.sh`:

```bash
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
            OUTPUT=$(hdfs dfs -ls / 2>&1)
            ;;
        2)
            DIR=$(whiptail --inputbox "Enter the HDFS directory path:" 10 60 "/" 3>&1 1>&2 2>&3)
            OUTPUT=$(hdfs dfs -ls "$DIR" 2>&1)
            ;;
        3)
            LOCAL_FILE=$(whiptail --inputbox "Enter the local file path:" 10 60 "" 3>&1 1>&2 2>&3)
            HDFS_PATH=$(whiptail --inputbox "Enter the HDFS destination path:" 10 60 "/" 3>&1 1>&2 2>&3)
            OUTPUT=$(hdfs dfs -put "$LOCAL_FILE" "$HDFS_PATH" 2>&1)
            ;;
        4)
            HDFS_FILE=$(whiptail --inputbox "Enter the HDFS file path:" 10 60 "/" 3>&1 1>&2 2>&3)
            LOCAL_PATH=$(whiptail --inputbox "Enter the local path to save the file:" 10 60 "." 3>&1 1>&2 2>&3)
            OUTPUT=$(hdfs dfs -get "$HDFS_FILE" "$LOCAL_PATH" 2>&1)
            ;;
        5)
            DIR_PATH=$(whiptail --inputbox "Enter the HDFS directory path to create:" 10 60 "/" 3>&1 1>&2 2>&3)
            PARENT_DIR=$(dirname "$DIR_PATH")
            if ! hdfs dfs -test -d "$PARENT_DIR"; then
                OUTPUT="Error: Parent directory $PARENT_DIR does not exist."
            else
                OUTPUT=$(hdfs dfs -mkdir "$DIR_PATH" 2>&1)
            fi
            ;;
        6)
            FILE_PATH=$(whiptail --inputbox "Enter the HDFS file path to remove:" 10 60 "/" 3>&1 1>&2 2>&3)
            OUTPUT=$(hdfs dfs -rm "$FILE_PATH" 2>&1)
            ;;
        7)
            exit 0
            ;;
    esac

    echo "$OUTPUT" > /tmp/hdfs_output.txt
    whiptail --title "Command Output" --textbox /tmp/hdfs_output.txt 25 80

else
    echo "You canceled the operation."
fi
```

Then run it by:

```bash
chmod +x hdfs_gui.sh
./hdfs_gui.sh
```

---

## Screenshots

### Menu Interface

![WhatsApp Image 2025-04-19 at 11 09 13 PM](https://github.com/user-attachments/assets/db9227d5-e80f-4998-b037-652a8eff5326)



![WhatsApp Image 2025-04-19 at 11 09 13 PM (1)](https://github.com/user-attachments/assets/b9ebf7e6-e2a3-46ca-9b54-585e450d3526)

---


