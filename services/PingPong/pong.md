# Pong
`Pong` stap in het ping pong proces.

**Request**
| Variable  | Type         | Verplicht?  | Toelichting         |
| --------  | -------      | --------- | -------               |
| name      | `String`     | Ja        | Naam van pong         |

**Response**
| Variable       | Type                   | Verplicht?  | Toelichting                       |
| --------       | -------                | ---------  | -------                            |
| results        | `PongResponse`         | Nee        | Resultaat van de ping stap         |
| is_last_step   | `bool`                 | Ja         | Indicatie van de laatste stap      |



**PongResponse**
| Variable  | Type                   | Verplicht?  | Toelichting                       |
| --------  | -------                | ---------  | -------                            |
| name      | `String`               | Nee        | Naam van pong                      |