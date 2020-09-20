# Serverspec

- Test수행을 간단하고 쉽게 하기위한 도구
- 인프라(서버) 설정 테스트 가능
- 테스트 항목에 대한 목록을 정해진 포맷을 기반으로 기술이 가능
- 테스트 결과를 리포트 형식으로 출력이 가능



## Ansible을 이용해서 Serverspec Install

1. **#1 rvm 및 ruby 설치**

   [vagrant@demo ansible-playbook-sample]$ `command curl -sSL https://rvm.io/mpapis.asc | sudo gpg2 --import -`

   ```
   gpg: directory `/root/.gnupg' created
   gpg: new configuration file `/root/.gnupg/gpg.conf' created
   gpg: WARNING: options in `/root/.gnupg/gpg.conf' are not yet active during this run
   gpg: keyring `/root/.gnupg/secring.gpg' created
   gpg: keyring `/root/.gnupg/pubring.gpg' created
   gpg: /root/.gnupg/trustdb.gpg: trustdb created
   gpg: key D39DC0E3: public key "Michal Papis (RVM signing) <mpapis@gmail.com>" imported
   gpg: Total number processed: 1
   gpg:               imported: 1  (RSA: 1)
   gpg: no ultimately trusted keys found
   ```

   [vagrant@demo ansible-playbook-sample]$ `command curl -sSL https://rvm.io/pkuczynski.asc | sudo gpg2 --import -`

   ```
   gpg: key 39499BDB: public key "Piotr Kuczynski <piotr.kuczynski@gmail.com>" imported
   gpg: Total number processed: 1
   gpg:               imported: 1  (RSA: 1)
   ```

   [vagrant@demo ansible-playbook-sample]$ `curl -L get.rvm.io | sudo bash -s stable`

   ```
     % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                    Dload  Upload   Total   Spent    Left  Speed
   100   194  100   194    0     0    340      0 --:--:-- --:--:-- --:--:--   340
   100 24535  100 24535    0     0  10527      0  0:00:02  0:00:02 --:--:-- 16488
   Downloading https://github.com/rvm/rvm/archive/1.29.10.tar.gz
   Downloading https://github.com/rvm/rvm/releases/download/1.29.10/1.29.10.tar.gz.asc
   gpg: Signature made Wed 25 Mar 2020 09:58:42 PM UTC using RSA key ID 39499BDB
   gpg: Good signature from "Piotr Kuczynski <piotr.kuczynski@gmail.com>"
   gpg: WARNING: This key is not certified with a trusted signature!
   gpg:          There is no indication that the signature belongs to the owner.
   Primary key fingerprint: 7D2B AF1C F37B 13E2 069D  6956 105B D0E7 3949 9BDB
   GPG verified '/usr/local/rvm/archives/rvm-1.29.10.tgz'
   Creating group 'rvm'
   Installing RVM to /usr/local/rvm/
   Installation of RVM in /usr/local/rvm/ is almost complete:
   
     * First you need to add all users that will be using rvm to 'rvm' group,
       and logout - login again, anyone using rvm will be operating with `umask u=rwx,g=rwx,o=rx`.
   
     * To start using RVM you need to run `source /etc/profile.d/rvm.sh`
       in all your open shell windows, in rare cases you need to reopen all shell windows.
     * Please do NOT forget to add your users to the rvm group.
        The installer no longer auto-adds root or users to the rvm group. Admins must do this.
        Also, please note that group memberships are ONLY evaluated at login time.
        This means that users must log out then back in before group membership takes effect!
   Thanks for installing RVM 🙏
   Please consider donating to our open collective to help us maintain RVM.
   
   👉  Donate: https://opencollective.com/rvm/donate
   ```

   [vagrant@demo ansible-playbook-sample]$ `sudo usermod -aG rvm $USER`

   [vagrant@demo ansible-playbook-sample]$`id $USER`

   ```
   uid=1000(vagrant) gid=1000(vagrant) groups=1000(vagrant),1001(rvm)
   ```

   [vagrant@demo ansible-playbook-sample]$ `source /etc/profile.d/rvm.sh`
   [vagrant@demo ansible-playbook-sample]$ `rvm reload`

   ```
   RVM reloaded!
   ```

   [vagrant@demo ansible-playbook-sample]$ `sudo su`
   [root@demo ansible-playbook-sample]# `rvm requirements run`

   ```
   Checking requirements for centos.
   Installing requirements for centos.
   Installing required packages: bison, libffi-devel, readline-devel, sqlite-devel, zlib-devel, openssl-devel............
   Requirements installation successful.
   ```

   [root@demo ansible-playbook-sample]# `rvm install 2.7`

   ```
   Searching for binary rubies, this might take some time.
   Found remote file https://rvm_io.global.ssl.fastly.net/binaries/centos/7/x86_64/ruby-2.7.0.tar.bz2
   Checking requirements for centos.
   Requirements installation successful.
   ruby-2.7.0 - #configure
   ruby-2.7.0 - #download
     % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                    Dload  Upload   Total   Spent    Left  Speed
   100 18.3M  100 18.3M    0     0  73231      0  0:04:22  0:04:22 --:--:-- 74837
   No checksum for downloaded archive, recording checksum in user configuration.
   ruby-2.7.0 - #validate archive
   ruby-2.7.0 - #extract
   ruby-2.7.0 - #validate binary
   ruby-2.7.0 - #setup
   ruby-2.7.0 - #gemset created /usr/local/rvm/gems/ruby-2.7.0@global
   ruby-2.7.0 - #importing gemset /usr/local/rvm/gemsets/global.gems..................................
   ruby-2.7.0 - #generating global wrappers.......
   ruby-2.7.0 - #gemset created /usr/local/rvm/gems/ruby-2.7.0
   ruby-2.7.0 - #importing gemsetfile /usr/local/rvm/gemsets/default.gems evaluated to empty gem list
   ruby-2.7.0 - #generating default wrappers.......
   ```

   [root@demo ansible-playbook-sample]# `rvm use 2.7 --default`

   ```\
   Using /usr/local/rvm/gems/ruby-2.7.0
   ```

   [root@demo ansible-playbook-sample]# `rvm list`

   ```
   =* ruby-2.7.0 [ x86_64 ]
   
   # => - current
   # =* - current && default
   #  * - default
   ```

   [root@demo ansible-playbook-sample]# `exit`

   [vagrant@demo ansible-playbook-sample]$ ruby -v

   ```
   ruby 2.0.0p648 (2015-12-16) [x86_64-linux]
   ```

   [vagrant@demo ansible-playbook-sample]$ `sudo ruby -v`

   ```
   ruby 2.0.0p648 (2015-12-16) [x86_64-linux]
   ```

   [vagrant@demo ansible-playbook-sample]$ `rvm use 2.7 --default`

   ```
   Using /usr/local/rvm/gems/ruby-2.7.0
   ```

   [vagrant@demo ansible-playbook-sample]$ `ruby -v`

   ```
   ruby 2.7.0p0 (2019-12-25 revision 647ee6f091) [x86_64-linux]
   ```

   [vagrant@demo ansible-playbook-sample]$ `sudo ruby -v`

   ```
   ruby 2.0.0p648 (2015-12-16) [x86_64-linux]
   ```

   [vagrant@demo ansible-playbook-sample]$ `which ruby`

   ```
   /usr/local/rvm/rubies/ruby-2.7.0/bin/ruby
   ```

   [vagrant@demo ansible-playbook-sample]$ `sudo which ruby`

   ```
   /bin/ruby
   ```

   [vagrant@demo ansible-playbook-sample]$ `sudo mv /bin/ruby /bin/ruby_2.0.0`
   [vagrant@demo ansible-playbook-sample]$ `sudo ln -s /usr/local/rvm/rubies/ruby-2.7.0/bin/ruby /bin/ruby`
   [vagrant@demo ansible-playbook-sample]$ `ruby -v`

   ```
   ruby 2.7.0p0 (2019-12-25 revision 647ee6f091) [x86_64-linux]
   ```

   [vagrant@demo ansible-playbook-sample]$ `sudo ruby -v`

   ```
   ruby 2.7.0p0 (2019-12-25 revision 647ee6f091) [x86_64-linux]
   ```

   

2. Playbook 파일(site.yml)에서 serverspec 롤을 추가

   ```yml
   ---
   - hosts: webservers
     become: yes
     connection: local
     roles:
       - common
       - nginx
       - serverspec
   #    - serverspec_sample
   #    - jenkins
   
   ```

3. serverspec 롤 확인

   `cat ./roles/serverspec/tasks/main.yml`

   ```
   # tasks file for serverspec
   - name: install ruby
     yum: name=ruby state=installed
   
   - name: install serverspec
     gem: name={{ item }} state=present user_install=no
     with_items:
      - rake
      - serverspec
   ```

4.  ansible-playbook으로 Serverspec 설치

   `ansible-playbook -i development site.yml --diff`

   ```
   [WARNING]: Invalid characters were found in group names but not replaced, use -vvvv to see
   details
   
   PLAY [webservers] ***************************************************************************
   
   TASK [Gathering Facts] **********************************************************************
   ok: [localhost]
   
   TASK [common : install epel] ****************************************************************
   ok: [localhost]
   
   TASK [install nginx] ************************************************************************
   ok: [localhost]
   
   TASK [nginx : replace index.html] ***********************************************************
   ok: [localhost]
   
   TASK [nginx start] **************************************************************************
   ok: [localhost]
   
   TASK [serverspec : install ruby] ************************************************************
   ok: [localhost]
   
   TASK [install serverspec] *******************************************************************
   ok: [localhost] => (item=rake)
   changed: [localhost] => (item=serverspec)
   
   PLAY RECAP **********************************************************************************
   localhost                  : ok=7    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
   ```

5. serverspec 초기 설정

   [vagrant@demo ansible-playbook-sample]$ `serverspec-init`

   ```
   Select OS type:
   
     1) UN*X
     2) Windows
   
   Select number: 1
   
   Select a backend type:
   
     1) SSH
     2) Exec (local)
   
   Select number: 2
   
    + spec/
    + spec/localhost/
    + spec/localhost/sample_spec.rb
    + spec/spec_helper.rb
    + Rakefile
    + .rspec
   ```

6. sample_spec.rb 파일을 확인

   [vagrant@demo ansible-playbook-sample]$ `cat ./spec/localhost/sample_spec.rb`

   - Server의 상태를 Test하는 코드

   ```
   require 'spec_helper'
   
   describe package('httpd'), :if => os[:family] == 'redhat' do
     it { should be_installed }	#	<= httpd가 설치되어 있어야 한다.
   end
   
   describe package('apache2'), :if => os[:family] == 'ubuntu' do
     it { should be_installed }
   end
   
   describe service('httpd'), :if => os[:family] == 'redhat' do
     it { should be_enabled }		#	<= httpd가 활성화되어 있어야 한다.
     it { should be_running }		#	<= httpd가 실행되고 있어야 한다.
   end
   
   describe service('apache2'), :if => os[:family] == 'ubuntu' do
     it { should be_enabled }
     it { should be_running }
   end
   
   describe service('org.apache.httpd'), :if => os[:family] == 'darwin' do
     it { should be_enabled }
     it { should be_running }
   end
   
   describe port(80) do
     it { should be_listening }		#	<= 80 포트가 동작(listen)하고 있어야 한다.
   end
   ```

7. serverspec을 이용한 Test 실행

   [vagrant@demo ansible-playbook-sample]$ `rake spec`

   ```
   /usr/local/rvm/rubies/ruby-2.7.0/bin/ruby -I/usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-support-3.9.3/lib:/usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-core-3.9.2/lib /usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-core-3.9.2/exe/rspec --pattern spec/localhost/\*_spec.rb
   
   Package "httpd"
     is expected to be installed (FAILED - 1)
   
   Service "httpd"
     is expected to be enabled (FAILED - 2)
     is expected to be running (FAILED - 3)
   
   Port "80"
     is expected to be listening
   
   Failures:			#	<= 실패한 테스트 케이스에 대한 상세 설명
   
     1) Package "httpd" is expected to be installed
        On host `localhost'
        Failure/Error: it { should be_installed }
          expected Package "httpd" to be installed
          /bin/sh -c rpm\ -q\ httpd
          package httpd is not installed
   
        # ./spec/localhost/sample_spec.rb:4:in `block (2 levels) in <top (required)>'
   
     2) Service "httpd" is expected to be enabled
        On host `localhost'
        Failure/Error: it { should be_enabled }
          expected Service "httpd" to be enabled
          /bin/sh -c systemctl\ --quiet\ is-enabled\ httpd
   
        # ./spec/localhost/sample_spec.rb:12:in `block (2 levels) in <top (required)>'
   
     3) Service "httpd" is expected to be running
        On host `localhost'
        Failure/Error: it { should be_running }
          expected Service "httpd" to be running
          /bin/sh -c systemctl\ is-active\ httpd
          unknown
   
        # ./spec/localhost/sample_spec.rb:13:in `block (2 levels) in <top (required)>'
   
   Finished in 0.11377 seconds (files took 0.99743 seconds to load)
   4 examples, 3 failures			#	<= 테스트 결과 요약
   
   Failed examples:		#	<= 실패한 테스트 케이스 목록
   
   rspec ./spec/localhost/sample_spec.rb:4 # Package "httpd" is expected to be installed
   rspec ./spec/localhost/sample_spec.rb:12 # Service "httpd" is expected to be enabled
   rspec ./spec/localhost/sample_spec.rb:13 # Service "httpd" is expected to be running
   
   /usr/local/rvm/rubies/ruby-2.7.0/bin/ruby -I/usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-support-3.9.3/lib:/usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-core-3.9.2/lib /usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-core-3.9.2/exe/rspec --pattern spec/localhost/\*_spec.rb failed
   ```



## Ansible에 Serverpec을 이용한 테스트 롤을 추가

1. Playbook 파일(site.yml)에 Serverspec 테스트 롤을 추가

   [vagrant@demo ansible-playbook-sample]$ `vim site.yml`

   ```
   ---
   - hosts: webservers
     become: yes
     connection: local
     roles:
       - common
       - nginx
       - serverspec                 
       - serverspec_sample           ⇐ 주석(#) 해제 후 저장
   #    - jenkins
   ```

2. **serverspec_sample 롤 정의 파일을 확인**

   [vagrant@demo ansible-playbook-sample]$ `cat ./roles/serverspec_sample/tasks/main.yml`

   ```
   ---
   # tasks file for serverspec_sample
   - name: distribute serverspec suite
     copy: src=serverspec_sample dest={{ serverspec_base_path }}	⇐ /tep 아래로 serverspec_sample 디렉터리를 복사
   
   - name: distribute spec file
     template: src=web_spec.rb.j2 dest={{ serverspec_path }}/spec/localhost/web_spec.rb	⇐ 템플릿에 정의된 내용으로 web_spec.rb 파일을 생성
     
   ```

   [vagrant@demo ansible-playbook-sample]$ `cat ./roles/serverspec_sample//vars/main.yml`

   - 위에서 사용되는 변수가 정의되어 있는곳

   ```yml
   serverspec_base_path: "/tmp"	⇐ task에서 사용하는 변수를 정의
   serverspec_path: "{{ serverspec_base_path }}/serverspec_sample"
   ```

   [vagrant@demo ansible-playbook-sample]$ `cat ./roles/serverspec_sample/templates/web_spec.rb.j2`

   ```
   require 'spec_helper'	⇐  serverspec에서 사용할 테스트 케이스 템플릿
   
   describe package('nginx') do	⇐ nginx 설치 여부
     it { should be_installed }
   end
   
   describe service('nginx') do	⇐ nginx 실행/활성화 여부
     it { should be_enabled }
     it { should be_running }
   end
   
   describe port(80) do			⇐ 80 포트 확인
     it { should be_listening }
   end
   
   describe file('/usr/share/nginx/html/index.html') do
     it { should be_file }			⇐ index.html 파일 존재 여부
     it { should exist }
     its(:content) { should match /^Hello, {{ env }} ansible!!$/ }	⇐ index.html 파일의 내용 검증
   end
   ```

3. ansible-playbook으로 spec 파일(테스트 케이스 파일)을 배포

   [vagrant@demo ansible-playbook-sample]$ `ansible-playbook -i development site.yml`

   ```yml
   [WARNING]: Invalid characters were found in group names but not replaced, use
   -vvvv to see details
   
   PLAY [webservers] **************************************************************
   
   TASK [Gathering Facts] *********************************************************
   ok: [localhost]
   
   TASK [common : install epel] ***************************************************
   ok: [localhost]
   
   TASK [install nginx] ***********************************************************
   ok: [localhost]
   
   TASK [nginx : replace index.html] **********************************************
   ok: [localhost]
   
   TASK [nginx start] *************************************************************
   ok: [localhost]
   
   TASK [serverspec : install ruby] ***********************************************
   ok: [localhost]
   
   TASK [install serverspec] ******************************************************
   ok: [localhost] => (item=rake)
   ok: [localhost] => (item=serverspec)
   
   # /tmp 디렉터리 아래로 serverspec_sample 디렉터리를 복사,  /tmp/serverspec_sample 디렉터리는 인프라가 원하는 형태로 구성되었는지 테스트하는 공간
   TASK [serverspec_sample : distribute serverspec suite] *************************
   changed: [localhost]
   
   TASK [serverspec_sample : distribute spec file] ********************************
   changed: [localhost]	⇐ 템플릿을 이용해서 web_spec.rb 파일을 정상적으로 생성
   
   PLAY RECAP *********************************************************************
   localhost                  : ok=9    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
   ```

4. spec 파일(테스트 케이스를 정의) 생성을 확인

   [vagrant@demo ansible-playbook-sample]$ `cat /tmp/serverspec_sample/spec/localhost/web_spec.rb`

   ```
   require 'spec_helper'
   
   describe package('nginx') do
     it { should be_installed }
   end
   
   describe service('nginx') do
     it { should be_enabled }
     it { should be_running }
   end
   
   describe port(80) do
     it { should be_listening }
   end
   
   describe file('/usr/share/nginx/html/index.html') do
     it { should be_file }
     it { should exist }
     its(:content) { should match /^Hello, development ansible!!$/ }
   end
   ```

5. 배포된 spec 파일을 이용해서 테스트를 실행

   [vagrant@demo ansible-playbook-sample]$ `cd /tmp/serverspec_sample/`

   [vagrant@demo serverspec_sample]$`rake spec`

   ```
   /usr/local/rvm/rubies/ruby-2.7.0/bin/ruby -I/usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-support-3.9.3/lib:/usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-core-3.9.2/lib /usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-core-3.9.2/exe/rspec --pattern spec/localhost/\*_spec.rb
   
   Package "nginx"
     is expected to be installed
   
   Service "nginx"
     is expected to be enabled
     is expected to be running
   
   Port "80"
     is expected to be listening
   
   File "/usr/share/nginx/html/index.html"
     is expected to be file
     is expected to exist
     content
       is expected to match /^Hello, development ansible!!$/ (FAILED - 1)
   
   Failures:
   
     1) File "/usr/share/nginx/html/index.html" content is expected to match /^Hello, development ansible!!$/
        On host `localhost'
        Failure/Error: its(:content) { should match /^Hello, development ansible!!$/ }
          expected "HELLO, development ansible !!!\n" to match /^Hello, development ansible!!$/
          Diff:
          @@ -1 +1 @@
          -/^Hello, development ansible!!$/
          +HELLO, development ansible !!!
   
          /bin/sh -c cat\ /usr/share/nginx/html/index.html\ 2\>\ /dev/null\ \|\|\ echo\ -n
          HELLO, development ansible !!!
   
        # ./spec/localhost/web_spec.rb:19:in `block (2 levels) in <top (required)>'
   
   Finished in 0.11232 seconds (files took 0.40656 seconds to load)
   7 examples, 1 failure
   
   Failed examples:
   
   rspec ./spec/localhost/web_spec.rb:19 # File "/usr/share/nginx/html/index.html" content is expected to match /^Hello, development ansible!!$/
   
   /usr/local/rvm/rubies/ruby-2.7.0/bin/ruby -I/usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-support-3.9.3/lib:/usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-core-3.9.2/lib /usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-core-3.9.2/exe/rspec --pattern spec/localhost/\*_spec.rb failed
   ```

6. 테스트 케이스를 통과하도록 컨텐츠를 수정 

   - **컨텐츠 형식을 정의하고 있는 템플릿 파일을 수정**

   [vagrant@demo ansible-playbook-sample]$ `cat ~/ansible-playbook-sample/roles/nginx/templates/index.html.j2`

   ```
   HELLO, {{ env }} ansible	⇐ 테스트 케이스와 상이 -> 테스트 케이스에 맞춰서 수정
   ```

   [vagrant@demo ansible-playbook-sample]$ `vim ~/ansible-playbook-sample/roles/nginx/templates/index.html.j2`

   ```
   Hello, {{ env }} ansible!!!
   ```

7. ansible-playbook으로 수정한 템플릿에 맞춰서 새롭게 index.html을 생성

   [vagrant@demo ansible-playbook-sample]$ `ansible-playbook -i development site.yml`

   ```yml
   [WARNING]: Invalid characters were found in group names but not replaced, use -vvvv to see details
   
   PLAY [webservers] *********************************************************************************
   
   TASK [Gathering Facts] ****************************************************************************
   ok: [localhost]
   
   TASK [common : install epel] **********************************************************************
   ok: [localhost]
   
   TASK [install nginx] ******************************************************************************
   ok: [localhost]
   
   TASK [nginx : replace index.html] *****************************************************************
   changed: [localhost]
   
   TASK [nginx start] ********************************************************************************
   ok: [localhost]
   
   TASK [serverspec : install ruby] ******************************************************************
   ok: [localhost]
   
   TASK [install serverspec] *************************************************************************
   ok: [localhost] => (item=rake)
   ok: [localhost] => (item=serverspec)
   
   TASK [serverspec_sample : distribute serverspec suite] ********************************************
   ok: [localhost]
   
   TASK [serverspec_sample : distribute spec file] ***************************************************
   ok: [localhost]
   
   PLAY RECAP ****************************************************************************************
   localhost                  : ok=9    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
   ```

8. 테스트 실행

   [vagrant@demo ansible-playbook-sample]$ `cd /tmp/serverspec_sample/`

   [vagrant@demo serverspec_sample]$ `rake spec`

   ```
   /usr/local/rvm/rubies/ruby-2.7.0/bin/ruby -I/usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-support-3.9.3/lib:/usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-core-3.9.2/lib /usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-core-3.9.2/exe/rspec --pattern spec/localhost/\*_spec.rb
   
   Package "nginx"
     is expected to be installed
   
   Service "nginx"
     is expected to be enabled
     is expected to be running
   
   Port "80"
     is expected to be listening
   
   File "/usr/share/nginx/html/index.html"
     is expected to be file
     is expected to exist
     content
       is expected to match /^Hello, development ansible!!$/
   
   Finished in 0.10557 seconds (files took 0.41014 seconds to load)
   7 examples, 0 failures			⇐ 7개 테스트 케이스를 모두 통과
   ```

9. nginx를 중지 후 테스트 실행

   [vagrant@demo serverspec_sample]$ `sudo systemctl stop nginx`
   [vagrant@demo serverspec_sample]$ `sudo systemctl status nginx`

   ```
   ● nginx.service - The nginx HTTP and reverse proxy server
      Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; vendor preset: disabled)
      Active: inactive (dead) since Thu 2020-09-10 08:13:20 UTC; 5s ago
    Main PID: 1597 (code=exited, status=0/SUCCESS)
   
   Sep 10 01:58:52 demo systemd[1]: Starting The nginx HTTP and ....
   Sep 10 01:58:53 demo nginx[1592]: nginx: the configuration fi...k
   Sep 10 01:58:53 demo nginx[1592]: nginx: configuration file /...l
   Sep 10 01:58:53 demo systemd[1]: Started The nginx HTTP and r....
   Sep 10 08:13:20 demo systemd[1]: Stopping The nginx HTTP and ....
   Sep 10 08:13:20 demo systemd[1]: Stopped The nginx HTTP and r....
   Hint: Some lines were ellipsized, use -l to show in full.
   ```

   [vagrant@demo serverspec_sample]$ `rake spec`

   ```
   /usr/local/rvm/rubies/ruby-2.7.0/bin/ruby -I/usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-support-3.9.3/lib:/usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-core-3.9.2/lib /usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-core-3.9.2/exe/rspec --pattern spec/localhost/\*_spec.rb
   
   Package "nginx"
     is expected to be installed
   
   Service "nginx"
     is expected to be enabled
     is expected to be running (FAILED - 1)
   
   Port "80"
     is expected to be listening (FAILED - 2)
   
   File "/usr/share/nginx/html/index.html"
     is expected to be file
     is expected to exist
     content
       is expected to match /^Hello, development ansible!!$/
   
   Failures:
   
     1) Service "nginx" is expected to be running
        On host `localhost'
        Failure/Error: it { should be_running }
          expected Service "nginx" to be running
          /bin/sh -c systemctl\ is-active\ nginx
          inactive
   
        # ./spec/localhost/web_spec.rb:9:in `block (2 levels) in <top (required)>'
   
     2) Port "80" is expected to be listening
        On host `localhost'
        Failure/Error: it { should be_listening }
          expected Port "80" to be listening
          /bin/sh -c ss\ -tunl\ \|\ grep\ -E\ --\ :80\\\
   
        # ./spec/localhost/web_spec.rb:13:in `block (2 levels) in <top (required)>'
   
   Finished in 0.11722 seconds (files took 0.41031 seconds to load)
   7 examples, 2 failures
   
   Failed examples:
   
   rspec ./spec/localhost/web_spec.rb:9 # Service "nginx" is expected to be running
   rspec ./spec/localhost/web_spec.rb:13 # Port "80" is expected to be listening
   
   /usr/local/rvm/rubies/ruby-2.7.0/bin/ruby -I/usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-support-3.9.3/lib:/usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-core-3.9.2/lib /usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-core-3.9.2/exe/rspec --pattern spec/localhost/\*_spec.rb failed
   ```

10. 테스트 결과를 HTML 형식으로 출력

    [vagrant@demo serverspec_sample]$ `sudo gem install coderay`

    [vagrant@demo serverspec_sample]$ `rake spec SPEC_OPTS="--format html" > ~/result.html`

    ```
    /usr/local/rvm/rubies/ruby-2.7.0/bin/ruby -I/usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-support-3.9.3/lib:/usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-core-3.9.2/lib /usr/local/rvm/rubies/ruby-2.7.0/lib/ruby/gems/2.7.0/gems/rspec-core-3.9.2/exe/rspec --pattern spec/localhost/\*_spec.rb failed
    ```

    [vagrant@demo serverspec_sample]$ `sudo mv ~/result.html /usr/share/nginx/html/`
    [vagrant@demo serverspec_sample]$ `sudo setenforce 0`
    [vagrant@demo serverspec_sample]$ `sudo systemctl start nginx.service`
    [vagrant@demo serverspec_sample]$ `sudo systemctl stop firewalld`

11. 호스트PC의 브라우저에서 `192.168.33.10/result.html`접속

    <img src="..\img\image-20200910172324897.png" alt="image-20200910172324897" style="zoom:67%;" />