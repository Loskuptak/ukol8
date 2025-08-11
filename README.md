# ukol7

## Popis
Ukol7 nginx aplikaci na AWS. Požit Terraform a Githubactions
custom nginx stránka   - jede
, konfigurace CloudWatch logů - jsou
 multi-AZ deployment - done

### Architektura
- AWS ECS Fargate cluster
- Application Load Balancer
- Multi-AZ
- CloudWatch logging
- ECR repository pro vlastní nginx image

### Custom nginx stránka
Pro vlastní obsah nginx stránky jsme vytvořili `index.html` a přidali jej do Docker image pomocí Dockerfile:

- Vytvořili jsme soubor `index.html` s vlastním HTML obsahem.
- Dockerfile založený na `nginx:alpine`, který kopíruje náš `index.html` do `/usr/share/nginx/html/`.
- Image jsme sestavili lokálně a pushli do AWS ECR repozitáře.
- Terraformem jsme nakonfigurovali ECS službu, aby používala tento vlastní image místo standardního nginx.
  
Tím jsme dosáhli, že nasazená aplikace zobrazuje náš vlastní obsah místo výchozí nginx stránky.


### Nasazení
1. Forkněte si tento repozitář
2. Nastavte GitHub Secrets ve vašem repozitáři:
   - `AWS_ACCESS_KEY_ID`
   - `AWS_SECRET_ACCESS_KEY`
3. Upravte backend konfiguraci Terraformu v `main.tf` (pokud používáte vzdálený state)
4. Push do větve `main` spustí GitHub Actions workflow, který provede:
   - Inicializaci Terraformu
   - Validaci konfigurace
   - Vytvoření/aktualizaci infrastruktury na AWS
   - Build a push vlastního nginx Docker image do ECR
   - Nasazení ECS služby s custom nginx image

### Použití
Po úspěšném nasazení získáte URL aplikace z výstupu GitHub Actions . Na této adrese poběží vlastní nginx stránka.

### Čištění
Pro odstranění nasazené infrastruktury spusťte:


terraform destroy
