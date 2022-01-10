# ansible-role-win-repo-app
Ansible role for installing and configuring a Windows exe from a GitHub release artifact

## Role Variables
Available variables are listed below, along with values from [defaults/main.yml](defaults/main.yml):
```yaml
app_repo: null
app_name: null
app_version: null
artifact_name: "{{ app_name }}-{{ app_version }}"
download_directory: "{{ ansible_user_dir }}\\Downloads"
extract_directory: "{{ download_directory }}\\{{ app_name }}-{{ app_version }}"
install_directory: "C:\\Program Files\\{{ app_name }}"
config_file: null
config_directory: null
config_file_path: null
```
When a `config_file` template is defined the `config_direcotry` and `config_file_path` variables must be set.

Required variables (role will fail if the variables are not set):
```yaml
app_repo: "string"
app_name: "string"
app_version: "string"
```

## Example Playbook
```yaml
- name: install app
  hosts: all

  handlers:
    - name: reboot
      ansible.windows.win_reboot:

  tasks:
    - name: install my-app
      ansible.builtin.include_role:
        name: ansible-role-win-repo-app
      vars:
        app_name: my-app
        app_version: v1.0.0
        app_repo: my-user/my-app
        config_file: config.json.j2
        config_directory: "{{ ansible_user_dir }}\\{{ app_name }}_config"
        config_file_path: "{{ config_directory }}\\config.json"
```
