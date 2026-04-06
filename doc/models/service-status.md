
# Service Status

## Structure

`ServiceStatus`

## Fields

| Name | Type | Tags | Description | Getter | Setter |
|  --- | --- | --- | --- | --- | --- |
| `app` | `?string` | Optional | - | getApp(): ?string | setApp(?string app): void |
| `moto` | `?string` | Optional | - | getMoto(): ?string | setMoto(?string moto): void |
| `notes` | `?int` | Optional | - | getNotes(): ?int | setNotes(?int notes): void |
| `users` | `?int` | Optional | - | getUsers(): ?int | setUsers(?int users): void |
| `time` | `?string` | Optional | - | getTime(): ?string | setTime(?string time): void |
| `os` | `?string` | Optional | - | getOs(): ?string | setOs(?string os): void |
| `phpVersion` | `?string` | Optional | - | getPhpVersion(): ?string | setPhpVersion(?string phpVersion): void |
| `status` | `string` | Required | - | getStatus(): string | setStatus(string status): void |

## Example (as JSON)

```json
{
  "app": "app2",
  "moto": "moto2",
  "notes": 138,
  "users": 58,
  "time": "time4",
  "status": "status6"
}
```

