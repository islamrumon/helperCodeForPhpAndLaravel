//.httaccess for boost laravel web applications 

      <IfModule mod_expires.c>
        ExpiresActive On

       # Images
        ExpiresByType image/jpeg "access plus 1 year"
        ExpiresByType image/gif "access plus 1 year"
        ExpiresByType image/png "access plus 1 year"
        ExpiresByType image/webp "access plus 1 year"
        ExpiresByType image/svg+xml "access plus 1 year"
        ExpiresByType image/x-icon "access plus 1 year"

        # Video
        ExpiresByType video/webm "access plus 1 year"
        ExpiresByType video/mp4 "access plus 1 year"
        ExpiresByType video/mpeg "access plus 1 year"

        # Fonts
        ExpiresByType font/ttf "access plus 1 year"
        ExpiresByType font/otf "access plus 1 year"
        ExpiresByType font/woff "access plus 1 year"
        ExpiresByType font/woff2 "access plus 1 year"
        ExpiresByType application/font-woff "access plus 1 year"

        # CSS, JavaScript
        ExpiresByType text/css "access plus 1 month"
        ExpiresByType text/javascript "access plus 1 month"
        ExpiresByType application/javascript "access plus 1 month"

        # Others
        ExpiresByType application/pdf "access plus 1 month"
        ExpiresByType image/vnd.microsoft.icon "access plus 1 year"
      </IfModule>


      <IfModule php7_module>
      php_flag display_errors Off
      php_value upload_max_filesize 10000000M
      php_value post_max_size 10000000M
      php_value max_execution_time 60000000M
      php_value max_input_time 10000000
      php_value memory_limit 10000000M
      php_value max_file_uploads 100
      php_value default_socket_timeout 60000000
      </IfModule>


// block the env file read HACK

      <Files ~ "\.(env|json|config.js|md|xml|gitignore|gitattributes|lock|editorconfig|yml|styleci.yml)$">
           Order allow,deny
           Deny from all
       </Files>
       
//this is cashing httaccess

      <IfModule mod_expires.c>
        ExpiresActive On

       # Images
        ExpiresByType image/jpeg "access plus 1 year"
        ExpiresByType image/gif "access plus 1 year"
        ExpiresByType image/png "access plus 1 year"
        ExpiresByType image/webp "access plus 1 year"
        ExpiresByType image/svg+xml "access plus 1 year"
        ExpiresByType image/x-icon "access plus 1 year"

        # Video
        ExpiresByType video/webm "access plus 1 year"
        ExpiresByType video/mp4 "access plus 1 year"
        ExpiresByType video/mpeg "access plus 1 year"

        # Fonts
        ExpiresByType font/ttf "access plus 1 year"
        ExpiresByType font/otf "access plus 1 year"
        ExpiresByType font/woff "access plus 1 year"
        ExpiresByType font/woff2 "access plus 1 year"
        ExpiresByType application/font-woff "access plus 1 year"

        # CSS, JavaScript
        ExpiresByType text/css "access plus 1 month"
        ExpiresByType text/javascript "access plus 1 month"
        ExpiresByType application/javascript "access plus 1 month"

        # Others
        ExpiresByType application/pdf "access plus 1 month"
        ExpiresByType image/vnd.microsoft.icon "access plus 1 year"
      </IfModule>
      
      
      Laragon local access another pc
      https://stackoverflow.com/questions/52388098/laragon-and-phpmyadmin-allow-access-outside
     
     
//this is the SFTP / FTP cdn file upload code
     
     public function uploadFiles(Request $request)
    {
        ini_set('max_execution_time', 180);
        $file_links = array();
        $url = 'http://37.59.39.135';
        $root = '//upload//tingtongfans//';
        if ($request->hasFile('files')) {
            foreach ($request->file('files') as $uploadFile) {
                //get filename with extension
                $fileNameWithExtension = $uploadFile->getClientOriginalName();
                //get filename without extension
                $filename = pathinfo($fileNameWithExtension, PATHINFO_FILENAME);
                //get file extension
                $extension = $uploadFile->getClientOriginalExtension();
                //filename to store
                $filenametostore = $filename . '_' . uniqid() . '.' . $extension;
                //Upload File to external server
                $data =   Storage::disk('custom-sftp')->put($filenametostore, fopen($uploadFile, 'r+'));
                //create the link
                array_push($file_links, $url . $root . $filenametostore);
            }
            $this->apiSuccess(ucfirst("success"), $file_links);
            return $this->apiOutput();
        } else {
            $this->apiSuccess(ucfirst("failed"), 'No such are file for upload');
            return $this->apiOutput();
        }
    }
