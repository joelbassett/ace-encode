#!/bin/bash

# READ IN SETTINGS FILE
CONFIG_FILE=/usr/bin/ace-encode/settings.conf

if [[ -f $CONFIG_FILE ]]; then
        . $CONFIG_FILE
fi

# FUNCTION : GET CURRENT TIMESTAMP
timestamp() {
  date +"%T"
}

# CHECK IF BLURAY FUNCTIONS ARE ENABLED
if [ "$ENABLE_BLURAY" != 1 ]; then
	exit;
fi

cd $BLURAY_OUTPUT_DIR

# FIND THE BLURAY TITLE
BLURAY_TITLE=$(blkid -o value -s LABEL $GLOBAL_SOURCE_DRIVE)

# CLEAN UP THE BLURAY TITLE (REPLACE SPACES WITH UNDERSCORES)
BLURAY_TITLE=${BLURAY_TITLE// /_}

# COMMENCE THE BACKUP OF THE BLURAY
if [ ! -d "$BLURAY_OUTPUT_DIR/$BLURAY_TITLE" ]; then

	# LOG : COMMENCING RIP OF THIS BLURAY
	echo "********************************* COMMENCING: $BLURAY_TITLE *********************************" >> $BLURAY_LOG_DIR/bluray-rip.log

	# XBMC-NOTIFY : COMMENCING
	if [ "$GLOBAL_ENABLE_XBMC" = 1 ]; then
        	xbmc-send --host=$GLOBAL_XBMC_HOST -a "Notification($BLURAY_RIPPER_NAME, Commencing backup $BLURAY_TITLE,20000)";
	fi

	# EMAIL-NOTIFY : COMMENCING
	if [ "$GLOBAL_ENABLE_EMAIL" = 1 ]; then
		echo "$(timestamp): COMMENCING: Commencing backup of $BLURAY_TITLE" | mail -s "$BLURAY_RIPPER_NAME : Commencing backup of $BLURAY_TITLE" $GLOBAL_EMAIL;
	fi

	# LOG : COMMENCING
	echo "$(timestamp) - COMMENCING: Commencing backup of $BLURAY_TITLE" >> $BLURAY_LOG_DIR/bluray-rip.log

	# START MakeMKV AND BACKUP THE BLURAY
	# makemkvcon backup disc:0 $BLURAY_OUTPUT_DIR/$BLURAY_TITLE --decrypt >> $BLURAY_LOG_DIR/$BLURAY_TITLE.log
	# makemkvcon --minlength=3600 -r --decrypt --directio=true mkv disc:0 all $BLURAY_OUTPUT_DIR/$BLURAY_TITLE | tee $BLURAY_LOG_DIR/$BLURAY_TITLE.log >> $BLURAY_LOG_DIR/bluray-encode.log
	makemkvcon backup --decrypt --directio=true --robot --cache=1024 --noscan -r --progress=-same disc:0 $BLURAY_OUTPUT_DIR/$BLURAY_TITLE >> $BLURAY_LOG_DIR/bluray-rip.log

	# XBMC-NOTIFY : COMPLETED
	if [ "$GLOBAL_ENABLE_XBMC" = 1 ]; then
        	xbmc-send --host=$GLOBAL_XBMC_HOST -a "Notification($BLURAY_RIPPER_NAME, Completed backup $BLURAY_TITLE,20000)";
	fi

	# EMAIL-NOTIFY : COMPLETED
	if [ "$GLOBAL_ENABLE_EMAIL" = 1 ]; then
		echo "$(timestamp): COMPLETED: Completed backup of $BLURAY_TITLE" | mail -s "$BLURAY_RIPPER_NAME : Completed backup of $BLURAY_TITLE" $GLOBAL_EMAIL;
	fi

	# LOG : COMPLETED
	echo "$(timestamp) - COMPLETED: Completed backup of $BLURAY_TITLE" >> $BLURAY_LOG_DIR/bluray-rip.log

	# EJECT THE DISC
	eject $GLOBAL_SOURCE_DRIVE >> $BLURAY_LOG_DIR/bluray-rip.log

	# LOG : COMPLETED ENCODE OF THIS BLURAY
	echo "*********************************  COMPLETED:  $BLURAY_TITLE  *********************************" >> $BLURAY_LOG_DIR/bluray-rip.log
 else
 	# LOG : RIPPED BLURAY FOLDER HAS ALREADY BEEN CREATED
        echo "$(timestamp) - ERROR: $BLURAY_TITLE has already been created" >> $BLURAY_LOG_DIR/bluray-rip.log

        # EJECT THE DISC
	eject $GLOBAL_SOURCE_DRIVE >> $BLURAY_LOG_DIR/bluray-rip.log

 fi
