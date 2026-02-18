# Zoho People - API REST

## Informations Générales

```
Base URL : https://people.zoho.com/people/api
Authentification : OAuth 2.0
Format : JSON / XML
Rate Limit : Selon le plan
```

## Authentification

```bash
curl -X POST "https://accounts.zoho.com/oauth/v2/token" \
  -d "grant_type=refresh_token" \
  -d "client_id=YOUR_CLIENT_ID" \
  -d "client_secret=YOUR_CLIENT_SECRET" \
  -d "refresh_token=YOUR_REFRESH_TOKEN" \
  -d "scope=ZOHOPEOPLE.forms.ALL,ZOHOPEOPLE.leave.ALL,ZOHOPEOPLE.attendance.ALL"
```

## Employés (Forms)

### Récupérer tous les employés
```bash
curl -X GET "https://people.zoho.com/people/api/forms/employee/getRecords" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -d "sIndex=1" \
  -d "limit=200"
```

### Récupérer un employé par ID
```bash
curl -X GET "https://people.zoho.com/people/api/forms/employee/getDataByID" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -d "recordId={employee_id}"
```

### Créer un employé
```bash
curl -X POST "https://people.zoho.com/people/api/forms/json/employee/insertRecord" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -H "Content-Type: application/json" \
  -d '{
    "inputData": {
      "FirstName": "Marie",
      "LastName": "Durand",
      "EmailID": "marie.durand@entreprise.fr",
      "Department": "Marketing",
      "Designation": "Responsable Marketing",
      "Dateofjoining": "01-Mar-2024",
      "EmployeeType": "Permanent",
      "Reporting_To": "EMP-042"
    }
  }'
```

### Mettre à jour un employé
```bash
curl -X POST "https://people.zoho.com/people/api/forms/json/employee/updateRecord" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -d '{
    "inputData": {
      "Designation": "Directrice Marketing",
      "Department": "Marketing Digital"
    },
    "recordId": "{employee_id}"
  }'
```

## Congés (Leave)

### Récupérer les soldes de congés
```bash
curl -X GET "https://people.zoho.com/people/api/leave/getLeaveBalance" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -d "employeeId={employee_id}"
```

**Réponse :**
```json
{
  "response": {
    "result": [
      {
        "leaveType": "Congés payés",
        "balance": 13,
        "used": 8,
        "pending": 5
      },
      {
        "leaveType": "RTT",
        "balance": 8,
        "used": 4,
        "pending": 0
      }
    ]
  }
}
```

### Soumettre une demande de congé
```bash
curl -X POST "https://people.zoho.com/people/api/leave/addLeave" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -d '{
    "employeeId": "{employee_id}",
    "leaveType": "Congés payés",
    "from": "2026-02-24",
    "to": "2026-02-28",
    "reason": "Vacances"
  }'
```

### Approuver / Refuser un congé
```bash
# Approuver
curl -X POST "https://people.zoho.com/people/api/leave/approveLeave" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -d "leaveId={leave_id}&action=approve"

# Refuser
curl -X POST "https://people.zoho.com/people/api/leave/approveLeave" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -d "leaveId={leave_id}&action=reject&reason=Période critique du projet"
```

## Présence (Attendance)

### Pointer (Check-in / Check-out)
```bash
# Check-in
curl -X POST "https://people.zoho.com/people/api/attendance" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -d '{
    "emailId": "marie.durand@entreprise.fr",
    "checkIn": "2026-02-18 09:00:00"
  }'

# Check-out
curl -X POST "https://people.zoho.com/people/api/attendance" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -d '{
    "emailId": "marie.durand@entreprise.fr",
    "checkOut": "2026-02-18 18:00:00"
  }'
```

### Récupérer la présence
```bash
curl -X GET "https://people.zoho.com/people/api/attendance/getUserReport" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -d "emailId=marie.durand@entreprise.fr" \
  -d "sdate=2026-02-01" \
  -d "edate=2026-02-28"
```

### Récupérer les feuilles de temps
```bash
curl -X GET "https://people.zoho.com/people/api/timetracker/getTimeLogs" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -d "user=marie.durand@entreprise.fr" \
  -d "from=2026-02-17" \
  -d "to=2026-02-21"
```

## Départements et Postes

### Lister les départements
```bash
curl -X GET "https://people.zoho.com/people/api/department/getAll" \
  -H "Authorization: Zoho-oauthtoken {access_token}"
```

### Lister les postes
```bash
curl -X GET "https://people.zoho.com/people/api/designation/getAll" \
  -H "Authorization: Zoho-oauthtoken {access_token}"
```

## Webhooks

### Événements Disponibles
```
employee.added
employee.updated
employee.deleted
leave.applied
leave.approved
leave.rejected
attendance.checkin
attendance.checkout
```

## Bonnes Pratiques API

1. **Utiliser la pagination** pour les listes longues (sIndex + limit)
2. **Mettre en cache** les données statiques (départements, postes)
3. **Gérer les rate limits** avec un système de retry
4. **Utiliser les webhooks** plutôt que le polling pour les événements
5. **Stocker les tokens de façon sécurisée** et rafraîchir proactivement
