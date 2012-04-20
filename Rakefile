## -- Rsync Deploy config -- ##
ssh_user       = "lokesh@lokesh.webfactional.com"
ssh_port       = "22"
document_root  = "~/webapps/lokeshdhakar/projects/lightbox2/"
rsync_delete   = true
deploy_default = "rsync"

## -- Misc Configs -- ##
sass_dir         = "sass"      # sass file directory
css_dir          = "css"       # compiled css directory
coffeescript_dir = "coffee"    # coffeescript file directory
js_dir           = "js"        # compiled js directory

desc "Watches for SASS and Coffeescript file changes to compile."
task :watch do
  puts "Starting to watch source with Compass and Coffeescript."
  compassPid = Process.spawn("compass watch --sass-dir #{sass_dir} --css-dir #{css_dir}")
  coffeePid = Process.spawn("coffee -o #{js_dir}/ -cw #{coffeescript_dir}/")
  trap("INT") {
    [compassPid, coffeePid].each { |pid| Process.kill(9, pid) rescue Errno::ESRCH }
    exit 0
  }

  [compassPid, coffeePid].each { |pid| Process.wait(pid) }
end



desc "Create zip of latest code."
task :zip, :version do |t, args|

  if args.version.nil?
    puts "You need to specify a version number."
    puts "Example: rake zip[2.5]"
    puts "Will create lightboxv2.5.zip"
  else
    puts "Zip-a-dee-doo-dah"

    puts "Version: #{args.version}"
    system "rm -rf lightbox"
    system "mkdir lightbox"
    system "cp index.html lightbox"
    system "cp README.markdown lightbox"
    system "cp -r css lightbox"
    system "cp -r js lightbox"
    system "cp -r images lightbox"
    system "zip -r lightbox#{args.version}.zip lightbox"
    system "rm -rf lightbox"

    puts "My, oh my what a wonderful day!"
  end
end


desc "Pushes code to production."
task :deploy do
  puts "Rsyncing files to production server."
  exclude = ""
  if File.exists?('./rsync-exclude')
    exclude = "--exclude-from '#{File.expand_path('./rsync-exclude')}'"
  end
  puts "## Deploying website via Rsync"
  ok_failed system("rsync -avze 'ssh -p #{ssh_port}' #{exclude} #{"--delete" unless rsync_delete == false} . #{ssh_user}:#{document_root}")
end

def ok_failed(condition)
  if (condition)
    puts "OK"
  else
    puts "FAILED"
  end
end
