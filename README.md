
//.httaccess redirect to http ti https

  <IfModule mod_rewrite.c>
  RewriteEngine on
  RewriteRule ^$ public/ [L]
  RewriteRule (.*) public/$1 [L]

  RewriteEngine On
  RewriteCond %{HTTPS} off
  RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
</IfModule>


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
        $url = 'http://ip';
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

//override or add env file with key and value\

            function overWriteEnvFile($type, $val)
            {
                $path = base_path('.env'); // get file ENV path
                if (file_exists($path)) {
                    $val = '"' . trim($val) . '"';
                    if (is_numeric(strpos(file_get_contents($path), $type)) && strpos(file_get_contents($path), $type) >= 0) {
                        file_put_contents($path, str_replace($type . '="' . env($type) . '"', $type . '=' . $val, file_get_contents($path)));
                    } else {
                        file_put_contents($path, file_get_contents($path) . "\r\n" . $type . '=' . $val);
                    }
                }
            }
            
//Hashtag link generat form textarea field  
            
        
        preg_match_all('/(?<!\w)#\w+/', $r->text_content, $allMatches);
        if (isset($allMatches[0]) && is_array($allMatches[0])) {
            foreach ($allMatches[0] as $item) {
                HashTag::updateOrCreate(['post_id' => $post->id, 'hashtag' => $item], []);
            }
        }

//hastag view on blade with purifer laravel
       
       {!! strip_tags(clean(nl2br($post->text_content)), ['youtube', 'a']) !!}

// FFMPG Compress the daynamic video file

    public function handle()
    {
        echo 'comment start';
        $videos = PostMedia::where('is_compressed', false)->where('media_type', 'Video')->latest()->first();
        ini_set('max_execution_time', 1800);
        // return $videos;
        if ($videos != null) {
            $mainPath = $videos->media_content;
            $path1 = public_path() . '/' . $mainPath;
            // $mainSourcePath = str_replace('/', '\\', $path1); // win
            $mainSourcePath = str_replace('\\', '/', $path1); //linux

            $compressPath = str_replace('storage/', 'storage/compress/', $videos->media_content);
            // $compressSource = str_replace('/', '\\', (public_path() . '/' . $compressPath));  //win
            $compressSource = str_replace('\\', '/', (public_path() . '/' . $compressPath));  //linux
            echo $mainPath;
            // if (file_exists('public/' . $mainPath)) {
            // CompressVideo::
            $bitrate   = '480k';
            // $command = "/usr/bin/ffmpeg -i $mainSourcePath -c:v libx264 -preset slow -crf 22 -c:a copy $compressSource";
            $command = "/usr/bin/ffmpeg -i $mainSourcePath -vcodec libx264 -vb 480k -r 24 -ab 64k -ar 48000 -ac 2 $compressSource";
            //-vcodec libx264 -crf 24 //optimize
            //-vcodec libx265 -crf 28 // compress
            // $command = "/usr/bin/ffmpeg -i $mainSourcePath -vcodec [libx265]  -crf 28 $compressSource";
            // $command = "C:/ffmpeg/bin/ffmpeg.exe -i $mainSourcePath -b $bitrate $compressSource";
            // $command = "C:/ffmpeg/bin/ffmpeg.exe -i $path -b:v $bitrate -bufsize $bitrate $path";
            Log::info("video compress --->   " . print_r($command, 1));
            system($command);
            // Log::info("data --->   " . print_r(system($command), 1));

            // ffmpeg.exe -y -i inp.mp4 -vcodec libx264 -vb 480k -r 24 -ab 64k -ar 48000 -ac 2 output.mp4
            $videos->mainfile_path =  $mainPath;
            $videos->is_compressed =  true;
            $videos->media_content =  $compressPath;
            $videos->save();
            echo "compress file save";

            // if (file_exists('public/' . $compressPath)) {

            // } else {
            //     echo "compress file is not found";
            // }
            // } else {
            //     echo "not found main file";
            // }
        }

        //convert time
        //convert end time
        //is_compressed,
        //main_file_path
    }
