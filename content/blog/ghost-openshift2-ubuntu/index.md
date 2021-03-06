---
title: Setting Up Ghost on OpenShift using Windows
date: "2014-05-15T12:12"
description: "DEPRECATED: Steps to install and configure Ghost CRM on OpenShift 2 from Windows 8, 7 or Vista."
---

![OpenShiftLogo](./OpenShift-2.jpeg)

Using Windows 7, assumed to work for Windows 8 and Vista.
</br>Run all code from the command line (cmd)."
</br>Insert your own values for `**CAPITALS**`.

</br>
###Initial Setup###

- Sign up with [OpenShift.com](http://openshift.com).
- Install [Ruby](http://rubyinstaller.org/downloads/). Check "Add Ruby executables to your PATH."
- Install [Git for Windows](http://msysgit.github.io/). Check "Use Git from the Windows Command Prompt."
- Verify Ruby and Git have installed properly. Any non-error output is fine.

  <pre><code>ruby -e 'puts "Welcome to Ruby"'
  git --version
  </code></pre>

- Install RHC using RubyGem.

  <pre><code>gem install rhc
  </code></pre>

- Setup RHC. Enter your OpenShift credentials and accept the defaults.

  <pre><code>rhc setup
  </code></pre>

- Set some git defaults.

  <pre><code>git config --global user.email "**EMAIL@ADDRESS.com**"
  git config --global user.name "**YOUR_NAME**"
  git config --global push.default simple
  </code></pre>

- Remember to update RHC from time to time

  <pre><code>gem update rhc
  </code></pre>

- Create the Ghost application. Pick a meaningful name.
  <pre><code>rhc app create **APP_NAME** nodejs-0.10 --env NODE_ENV=production --from-code https://github.com/openshift-quickstart/openshift-ghost-quickstart.git
  </code></pre>

</br>
###Troubleshooting the Setup###

- The first time this is done the authenticity of your site likely can't be established, so just type 'yes' when prompted. If setup get stuck after this, press Ctrl+C and clone the git repository manually.

  <pre><code>rhc git-clone **APP_NAME**</code></pre>

- If you don't make it that far or want to start fresh, delete the application and free up that app name.

<pre><code>rhc app-delete **APP_NAME** --confirm</code></pre>

</br>
###Working with Git and RHC###

- Navigate into the new directory. Edit the files there manually.

<pre><code>cd **APP_NAME**</code></pre>

- To see info about your app type the following.

  <pre><code>rhc show-app **APP_NAME**
  </code></pre>

- Update and push out changes like this.

  <pre><code>git add --all .
  git commit -m 'My changes'
  git push
  </code></pre>

- Then restart the app to put those changes into effect.

  <pre><code>rhc app restart
  </code></pre>

- Don't forget to take regular snapshots!
  <pre><code>rhc save-snapshot
  </code></pre>

</br>
###Themes###

- Download and copy themes into \*\*APP_FOLDER\*\*/content/themes. \* Apply changes by updating git and restarting the gear.
  - Downloads are available from these sites. Most are paid.
    </br>https://ghost.org/forum/themes/
    </br>http://themeforest.net/category/blogging/ghost-themes

</br>
###Configuring Ghost###

- Login to your new blog from the following link.
  </br>http://\*\*APP\_NAME\*\*-\*\*OPENSHIFT\_NAME\*\*.rhcloud.com/ghost
  - Create an administrator account on the first login. If something goes wrong restart the app and try again.
  - The rest is fairly straightfordward, so you can take it from here.

</br>

- [OPTIONAL] Provide a Gmail account for sending password resets.
  _ Edit config.js, but be warned this is not secure!
  _ After this is done update git and restart the app.
  <pre><code>        url: 'http://**YOUR_SITE_ADDRESS**,
          mail: {
                  transport: 'SMTP',
                  options: {
                          service: 'Gmail',
                          auth: {
                                  user: '**GMAIL@GMAIL.COM',
                                  pass: '**PASSWORD**'
                          }
                  }
          },
  </code></pre>

</br>
###Custom Domain Setup###

- Visit your application settings on OpenShift and click 'change.'
- Use the guide to changing Internet domain registrar's cname records.
  [https://openshift.redhat.com/app/console/applications](https://openshift.redhat.com/app/console/applications)

###Configuring GhostWall Theme###

For details on this this site is configured, please see the link below. Be warned that the theme used costs around \$24 US.

http://www.derekroberts.ca/build/

---

That's it!

---

Many thanks to [Andrew Hobden](http://www.hoverbear.org) (aka. Hoverbear) for providing just enough taunting/direction to help.
