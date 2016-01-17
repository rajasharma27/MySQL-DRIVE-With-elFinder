# MySQL-DRIVE-With-elFinder
MySQL DRIVE With Set Attributes (read, write, locked, hidden) On Files or Folders

First you create "elfinder_file" table inside MySQL elfinderdatabase

Set Attributes (read, write, locked, hidden) On Files or Folders with elFinderVolumeMySQL.class.php

You can set attribute for files and folder in side MySQL Database drive

Set default attribute
'defaults'    => array('read' => true, 'write' => true, 'locked' => false, 'hidden' => false)

Set file or folder attribute

'attributes' => array(
                array(				
                'pattern'	=> 'demo/^$/', // Dont write or delete to this and all subfolders
                'read'		=> true,
                'write'		=> false,
                'locked'	=> true				
                 )


