### 相同變數名稱在 Roles, Role_var, 與 Playbooks 的優先使用順序

[Using Variables — Ansible Documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html#understanding-variable-precedence)

<aside>
💡 上面文件列的優先權是 “最小到最大喔” (from least to greatest)

</aside>

完整範例：

Playbook (**`roles-var-example.yml`**):

```yaml
- name: Example playbook
  hosts: localhost
  vars:
    my_var: "我是 Playbook 中的變數"

  roles:
    - role: my_role
      vars:
        my_var: "我是 roles 額外定義的變數"
```

 角色`my_role`目錄結構：

```yaml
my_role/
  - vars/
    - main.yml
  - tasks/
    - main.yml
```

角色 **`my_role`** 的 **`vars/main.yml`** 內容：

```yaml
my_var: "我是 my_role 中 **vars/main.yml** 的變數"
```

角色 **`my_role`** 的 **`tasks/main.yml`** 內容：

```yaml
- name: "Print my_var"
  debug:
    msg: "{{ my_var }}"
```

大家可以練習看看，驗證下執行結果：

`我是 my_role 中 vars/main.yml 的變數` > `我是 roles 額外定義的變數` > `我是 Playbook 中的變數`