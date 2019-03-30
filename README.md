# tagging-resources

# README #

Best Practices AWS to tag resources:

     https://d0.awsstatic.com/aws-answers/AWS_Tagging_Strategies.pdf
       
### Contenido ###

Se crea los siguientes grupos:

- ReadOnlyGroup
- SecurityAuditGroup
- DeveloperUserGroup
- NetworkAdminGroup
- SystemAdminGroup
- LNKPolicyBilling
- LNKPolicySupport
- LNKPolicyUserSelfServiceIAM

### Utilidades ###
    
Para usuarios que tienen más de una cuenta, crear un salto centralizado.

1. Lanzamos multi-account.yaml cuenta Origen (cuenta de salto)

   - Stack name: <source-account-name>-<destination-account-name>-iam
   - SourceSwitchRole: <source-account-number>
   - ClientName: <destination-account-name> "La cuenta a la que saltaremos"
   - CuentaDestino: NO
   - CuentaOrigen: YES
   - DestinationSwitchRole: <source-account-name> "La cuenta de salto"

2. Lanzamos multi-account.yaml cuenta Destino (cuenta de salto)

   - Stack name: <source-account-name>-<destination-account-name>-iam
   - SourceSwitchRole: <source-account-number>
   - ClientName: <destination-account-name> "La cuenta a la que saltaremos"
   - CuentaDestino: YES
   - CuentaOrigen: NO
   - DestinationSwitchRole: <source-account-name> "La cuenta de salto"

## Descripcion ##

### ROLE ###

El Role se crea en la cuenta a la que queremos saltar <Cuenta-Destino-Salto>

 1.  Create role 
 2. Entity - Another AWS Account
     -  Account ID: <Cuenta-Origen-Salto>
 3. Attach permissions policies 
     - AWS Managed: Si la politica puede ser cubierta por las administradas por AWS, seleccionar esas.
     - Customer Managed: Si la política tiene alguna particularidad, crear una custom.
 4. Review  
     - Role name: <NombreGrupo>Role
     - Role description: <NombreGrupo>Role

### Inline Policies ### 

Se añade automáticamente la "Inline Policy" al grupo de la <Cuenta-Origen-Salto>, que se le va a dar el permiso de hacer el salto.

NombreGrupo-CrossAccountSignin-Policy
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": [
                "arn:aws:iam::<Cuenta-Destino-Salto-1>:role/NombreRole",
            ]
        }
    ]
}