# MySQL-DRIVE-With-elFinder
MySQL DRIVE With Set Attributes (read, write, locked, hidden) On Files or Folders

Set Attributes (read, write, locked, hidden) On Files or Folders with elFinderVolumeMySQL.class.php

// Documentation for connector options:
// https://github.com/Studio-42/elFinder/wiki/Connector-configuration-options
$opts = array(
	// 'debug' => true,
	'roots' => array(
		array(
			'driver'        => 'LocalFileSystem',           // driver for accessing file system (REQUIRED)
			'path'          => '../files/',                 // path to files (REQUIRED)
			'alias'			=>	'Home',
			'URL'           => dirname($_SERVER['PHP_SELF']) . '/../files/', // URL to files (REQUIRED)
			'uploadDeny'    => array('all'),                // All Mimetypes not allowed to upload
			'uploadAllow'   => array('image', 'text/plain'),// Mimetype `image` and `text/plain` allowed to upload
			'uploadOrder'   => array('deny', 'allow'),      // allowed Mimetype `image` and `text/plain` only
			'accessControl' => 'access'                     // disable and hide dot starting files (OPTIONAL)
		),
		 array (
				'driver'  		=> 'MySQL',
				'host' 	  		=> 'localhost',
				'port'    		=> 3306,											
				'user' 	  		=> 'root',
				'pass' 	  		=> 'your password',
				'db' 	  		  => 'elFinderDatabase',
				'files_table' => 'elfinder_file',
				'alias'			  => 'MySQL DATABASE',
				'files_table' => 'elfinder_file',
				'path'        => 1,
				'tmpPath' 		=> '../files/.tmp',
				'tmbPath' 		=> '../files/.tmb',
				'tmbURL'  		=> dirname($_SERVER['PHP_SELF']) . '/../files/.tmb',
				'defaults'    => array('read' => true, 'write' => true, 'locked' => false, 'hidden' => false),
				'attributes' => array(
						array(				
							'pattern'	=> 'demo/^$/', // Dont write or delete to this and all subfolders
							'read'		=> true,
							'write'		=> true,
							'locked'	=> true				
							),
						array(
							'pattern' => 'demo/^.png$/', //You can also set permissions for file types by adding, for example, .png inside pattern.
							'read'    => true,
							'write'   => true,
							'locked'  => false
						),
						array(
							'pattern' => 'demo/^MyFile.txt$/', //You can also set permissions inside folder pattern.
							'read'    => true,
							'write'   => false,
							'locked'  => true
						)
					)
				)	
	
	)
);

//---------------------------------------------------------------
// Create MySQL table elfinder_file inside elfinder database

DROP TABLE IF EXISTS `elfinder_file`;
CREATE TABLE IF NOT EXISTS `elfinder_file` (
  `id`        int(7) unsigned NOT NULL auto_increment,
  `parent_id` int(7) unsigned NOT NULL,
  `name`      varchar(256) NOT NULL,
  `content`   longblob NOT NULL,
  `size`      int(10) unsigned NOT NULL default '0',
  `mtime`     int(10) unsigned NOT NULL,
  `mime`      varchar(256) NOT NULL default 'unknown',
  `read`      enum('1', '0') NOT NULL default '1',
  `write`     enum('1', '0') NOT NULL default '1',
  `locked`    enum('1', '0') NOT NULL default '0',
  `hidden`    enum('1', '0') NOT NULL default '0',
  `width`     int(5) NOT NULL,
  `height`    int(5) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY  `parent_name` (`parent_id`, `name`),
  KEY         `parent_id`   (`parent_id`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8 COLLATE=utf8_general_ci;

INSERT INTO `elfinder_file`
(`id`, `parent_id`, `name`,     `content`, `size`, `mtime`, `mime`,      `read`, `write`, `locked`, `hidden`, `width`, `height`) VALUES 
('1',  '0',         'DATABASE', '',        '0',    '0',     'directory', '1',    '1',     '0',      '0',      '0',     '0');

