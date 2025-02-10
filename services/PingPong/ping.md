# Ping
`Ping` stap in het ping pong proces.

**Request**
| Variable  | Type         | Verplicht?  | Toelichting         |
| --------  | -------      | --------- | -------               |
| name      | `String`     | Ja        | Naam van de aanvrager |

**Response**
| Variable  | Type                   | Verplicht?  | Toelichting                       |
| --------  | -------                | ---------  | -------                            |
| results   | `PingResponse`         | Nee        | Resultaat van de ping stap         |

**PingResponse**
| Variable  | Type                   | Verplicht?  | Toelichting                       |
| --------  | -------                | ---------  | -------                            |
| name      | `String`               | Nee        | Naam van de aanvrager              |