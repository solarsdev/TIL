# 핸들러 (Handler)

## 핸들러가 요구되는 배경

- 앤서블을 이용해서 서버의 설정 파일을 변경
- 설정 파일이 변경되더라도 해당 설정 파일은 서비스를 시작할 때 읽어들이는 구조로 되어 있음
- 파일 변경 이후에 해당 파일과 연결된 서비스를 재시작 하는 등의 추가적인 처리가 동적으로 필요
- 동적 처리를 이벤트 드리븐으로 처리하기 위해 핸들러가 필요함

## 이벤트 핸들러

```yaml
---

- name: Example
  hosts: ubuntu
  become: true
  tasks:
  # Docs: https://docs.ansible.com/ansible/latest/collections/ansible/builtin/user_module.html
  - name: "Create a user"
    user: "name=fastcampus shell=/bin/bash"

  - name: "Hello World"
    command: "echo 'Hello World!'"

  # Docs: https://docs.ansible.com/ansible/latest/collections/ansible/builtin/lineinfile_module.html
  - name: "Add DNS server to resolv.conf"
    lineinfile:
      path: /etc/resolv.conf
      line: 'nameserver 8.8.8.8'

  # Docs: https://docs.ansible.com/ansible/latest/collections/ansible/builtin/apt_module.html
  - name: "Install Nginx"
    apt:
      name: nginx
      state: present
      update_cache: true

  # Docs: https://docs.ansible.com/ansible/latest/collections/ansible/posix/synchronize_module.html
  - name: "Upload web directory"
    synchronize:
      src: files/html/
      dest: /var/www/html
      archive: true
      checksum: true
      recursive: true
      delete: true

  # Docs: https://docs.ansible.com/ansible/latest/collections/ansible/builtin/copy_module.html
  - name: "Copy nginx configuration file"
    copy:
      src: files/default
      dest: /etc/nginx/sites-enabled/default
      owner: "{{ ansible_user }}"
      group: "{{ ansible_user }}"
      mode: '0644'
    notify:
    - Restart Nginx

  # Docs: https://docs.ansible.com/ansible/latest/collections/ansible/builtin/service_module.html
  - name: "Ensure nginx service started"
    service:
      name: nginx
      state: started

  handlers:
  - name: Restart Nginx
    service:
      name: nginx
      state: restarted
```

- 중요한 파트

```yaml
notify:
- Restart Nginx

handlers:
- name: Restart Nginx
	service:
		name: nginx
		state: restarted
```
