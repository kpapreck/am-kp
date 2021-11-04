*****************************************************
Molecule based development/testing installation guide
*****************************************************

Requirements
============

* A host where the tester/developer develops/tests the role from (probably your laptop or the VDI)
* Docker environment installed on that host (and access to physical servers for some scenarios)
* Python3.x installed on that hos
    * MacOS: ``brew install python3``
* Clone the repository from the Git based Source Control System (Github.com or otherwise)

Install virtual env + required Python dependencies
==================================================

.. code-block:: bash

    $ git pull https://<path_to_repo>
    $ cd ansible
    $ echo "export DOCKER_REGISTRY=[registry.docker.io|company_specific_repo_mirror]" >> ~/.bash_profile
    $ echo "export SF_CUSTOM_INVENTORY=[molecule directory-relative path to custom inventory file; ../resources/inventory/default-inventory.yml]" >> ~/.bash_profile
    $ source ~/.bash_profile
    $ make prepare

IF applicable:

- Set path to custom host_vars and group_vars directories (see molecule/resources/inventory)
- Create Ansible-Vault and/password file (see "Using secure SSH passwords" in `README.md </README.md>`_)

Development workflow with Molecule
----------------------------------

When updating this role, your development flow should follow something along the lines of:

1. Ensure there is a test in `./molecule/default/tests/` that covers the change you're making.
2. If not, create a test or scenario to cover your planned update
3. Update the role with your planned change
4. Run ``make lint-role ROLE_NAME=<path_to_role>`` and fix any issues uncovered
5. Run ``make apply-role ROLE_NAME=<path_to_role>`` and fix any issues uncovered for the default scenario
6. OR, run ``make apply-role-scenario ROLE_NAME=<role_name> SCENARIO=<scenario_name>`` and fix any issues uncovered for the specified scenario
7. Run ``make verify-role ROLE_NAME=<path_to_role>`` and fix any issues uncovered for the default scenario
8. OR, run ``make verify-role-scenario ROLE_NAME=<role_name> SCENARIO=<scenario_name>`` and fix any issues uncovered for the specified scenario
9. Run ``make test-role-scenario ROLE_NAME=<path_to_role SCENARIO=<molecule_scenario_directory_name>`` to run through the test harness for the role/scenario
10. OR, run ``make test-role ROLE_NAME=<path_to_role>`` to run through the entire test harness for this role
