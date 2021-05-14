# How to create custom credential in AWX and use them in playbook. In this example we will create HashiCorp Vault Credential and use in our automation.

1. Create new credential type in AWX. (This option is visible in AWX only using administrative account)
   
   - Name: Provide meaning full name to your new credential type
   - Description: Add description for your custom credential type
   - Input Configuration:

     ```
        fields:
         - id: vault_address
           type: string
           label: URL to Vault Server
         - id: vault_namespace
           type: string
           label: Vault Namespace
         - id: vault_role_id
           type: string
           label: Vault Role ID
           secret: true
         - id: vault_secret_id
           type: string
           label: Vault Secret ID
           secret: true
         - id: vault_path
           type: string
           label: Vault Secret Path
       required:
         - vault_address
         - vault_namespace
         - vault_role_id
         - vault_secret_id
         - vault_path
     ```
     
   - Injector Configuration:

     ```
        extra_vars:
          VAULT_ADDR: '{{ vault_address }}'
          VAULT_NAMESPACE: '{{ vault_namespace }}'
          VAULT_PATH: '{{ vault_path }}'
          VAULT_ROLE_ID: '{{ vault_role_id }}'
          VAULT_SECRET_ID: '{{ vault_secret_id }}'
     ```

2. Create Credential object using custom credenntial type you created.

3. Create a job template and assign credential you created in #2. Injector configuration varibales will be available to your playbook as extra args.
   I am using playbook like below:
   
   ```
      ---
      - name: Get vault details from AWX
        hosts: all
        gather_facts: false
        tasks:
          - debug:
              var: VAULT_ADDR

          - debug:
              var: VAULT_NAMESPACE

          - debug:
              var: VAULT_ROLE_ID

          - debug:
              var: VAULT_SECRET_ID

          - debug:
              var: VAULT_PATH
   ```


