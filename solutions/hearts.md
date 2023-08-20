[![CICD-SEC-2 Inadequate Identity and Access Management](https://img.shields.io/badge/CICD--SEC--2-Inadequate%20Identity%20and%20Access%20Management-brightgreen)](https://owasp.org/www-project-top-10-ci-cd-security-risks/CICD-SEC-02-Inadequate-Identity-And-Access-Management)

Identify the user that has privileged access to manage agents. Looking at the list of Jenkins users shows a short list of users, one of them is Knave - whose description reveals that it is an agents admin. Sounds like a place to start from.

The users in the Jenkins instance are managed by Jenkins’ own user database, which lacks basic security controls against various types of attacks. It means that you can brute force the Knave user to find its password. Then, you’d be able to create a new agent and exfiltrate the System credentials by making Jenkins send it to your server.


1. Login to Jenkins with alice.
2. Browse to the People page on the top left corner and click on Knave to read its description, which reveals that it has permissions to manage Jenkins agents.

    ![hearts_1](../images/hearts_1.png "hearts_1")

3. Brute force Knave’s password on Jenkins. You can use the Rockyou list. Password is _rockme_. Login with Knave.

4. Click one of the agents on the left panel, then click Nodes at the top bar and create a new node.

5. Setup an SSH server that can log credentials on login attempts, using a project like [this](https://github.com/jtesta/ssh-mitm). Guide for installing the tool can be found [here](https://miloserdov.org/?p=3699).

6. Configure a new node with the following settings:

    ![hearts_2](../images/hearts_2.png "hearts_2")

7. Read the logs using
`sudo tail -f /var/log/auth.log`

    ![hearts_3](../images/hearts_3.png "hearts_3")

Note: The SSH server can also be created locally by setting up the ssh-mitm container in the "goat" network using `docker network connect`. 
