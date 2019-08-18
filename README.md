# Vagrant test environment

This branch contains a test environment for the mariadb role, powered by Vagrant.

I use [git-worktree(1)](https://git-scm.com/docs/git-worktree) to include the test code into the working directory. Instructions for running the tests:

1. Fetch the tests branch: `git fetch origin vagrant-tests`
2. Create a Git worktree for the test code: `git worktree add vagrant-tests vagrant-tests` (remark: this requires at least Git v2.5.0). This will create a directory `vagrant-tests/`.
3. `cd vagrant-tests/`
4. `vagrant up` will then create a VM for each supported platform and apply the test playbook, `test.yml`. See the table below for an overview of available test VMs.
5. Run the functional tests using the provided BATS test script, `mariadb.bats`:

    ```console
    $ SUT_IP=192.168.56.10 bats mariadb.bats 
    ✓ Root user should not be able to run a query remotely
    ✓ User should be able to run SHOW TABLES on own database
    ✓ User should not be able to run SHOW TABLES on other database
    ✓ User should be able to run SELECT * FROM TABLE on own database
    ✓ User should not be able to run SELECT * FROM TABLE on other database
    ✓ User should be able to run UPDATE query on own database
    ✓ User should not be able to run UPDATE query on other database

    7 tests, 0 failures
    ```

    The variable `SUT_IP` contains the IP address of the *system under test*, i.e. the VM that you want to run the tests on.

## Test VMs

| Name      | Distro     | IP address    |
| :---      | :---       | :---          |
| srvcentos | CentOS 7.6 | 192.168.56.10 |
| srvfedora | Fedora 30  | 192.168.56.11 |
