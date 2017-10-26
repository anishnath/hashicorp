
vault server  -config=vault.hcl

export VAULT_ADDR='http://127.0.0.1:8200'

Unseal Key: hK33+pZyY1UKyII7r+iRdSxXKhWnAAdeiCPpycOEp9U=
Root Token: 260c5433-a7c3-a6b1-9b16-8c3d253d0f8e

vault mount ssh

vault write ssh/roles/otp_key_role \
    key_type=otp \
    default_user=root \
    cidr_list=10.0.2.1/8


vault write ssh/creds/otp_key_role ip=10.0.2.15

vim /etc/pam.d/sshd
#auth requisite pam_exec.so quiet expose_authtok log=/tmp/vaultssh.log /opt/vault-ssh-helper -config=/opt/config.hcl -dev
#auth optional pam_unix.so not_set_pass use_first_pass nodelay

vim /etc/ssh/sshd_config
ChallengeResponseAuthentication yes
UsePAM yes
PasswordAuthentication no

vault unseal -address=${VAULT_ADDR} Mc674tyOk9rjLv8I8atrYy9i3+nnyPv7VxtEvlGpB1Yi
vault unseal -address=${VAULT_ADDR} AcvCd3kiiG7LEfBUdxBLe4HmWNpumfjJoq8xt/KtAzoL
vault unseal -address=${VAULT_ADDR} 8TlSi0dn09ELCEEgNYR2kcxKtk/Vnkcwo39U/8nOXUoB

Unseal Key 1: Mc674tyOk9rjLv8I8atrYy9i3+nnyPv7VxtEvlGpB1Yi
Unseal Key 2: AcvCd3kiiG7LEfBUdxBLe4HmWNpumfjJoq8xt/KtAzoL
Unseal Key 3: 8TlSi0dn09ELCEEgNYR2kcxKtk/Vnkcwo39U/8nOXUoB
Unseal Key 4: eKaLw6wTjsl69vyqqY+FBtNMFtfFVjnMpuHSVPLt/n8k
Unseal Key 5: X1eqqCNy2r/BZ2/vLlo/Nn4TBEPTxyZyZmvILJFkmoQ9
Initial Root Token: a3065f08-f075-6381-27f9-0b215f543a61

vault auth -address=${VAULT_ADDR} a3065f08-f075-6381-27f9-0b215f543a61

