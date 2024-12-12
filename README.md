# Plataforma de Alquiler de Coches Inteligente

es una aplicación web y móvil que revoluciona la experiencia de alquiler de vehículos, permitiendo a los usuarios encontrar, reservar y gestionar alquileres de coches de manera rápida, segura y completamente personalizada.

## Características principales

- Búsqueda de vehículos por ubicación y fecha
- Filtrado avanzado por tipo de vehículo, precio y características
- Reservas instantáneas con confirmación inmediata

### Tecnologías utilizadas

| Componente | Tecnología |
|------------|------------|
| Frontend   | html/css |
| Backend    | PHP |
| Base de datos | MySQL |
| Autenticación | PHP Sessions |

link pagina web[Automotive Rental Trends](https://www.alquilerdecoches.com/)

![RoadRover Logo](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b2/Database-mysql.svg/724px-Database-mysql.svg.png)

## Ejemplo de código: Función de reserva de vehículo

```php
<?php
function bookVehicle($userId, $vehicleId, $startDate, $endDate) {
    global $conn;
    
    try {
        // Obtener información del vehículo
        $stmt = $conn->prepare("SELECT * FROM vehicles WHERE id = ?");
        $stmt->bind_param("i", $vehicleId);
        $stmt->execute();
        $result = $stmt->get_result();
        $vehicle = $result->fetch_assoc();
        
        // Calcular precio total
        $totalPrice = calculateRentalPrice($vehicle, $startDate, $endDate);
        
        // Insertar reserva en la base de datos
        $stmt = $conn->prepare("INSERT INTO bookings (user_id, vehicle_id, start_date, end_date, total_price) VALUES (?, ?, ?, ?, ?)");
        $stmt->bind_param("iissd", $userId, $vehicleId, $startDate, $endDate, $totalPrice);
        $stmt->execute();
        
        $bookingId = $stmt->insert_id;
        
        return [
            'booking_id' => $bookingId,
            'total_price' => $totalPrice
        ];
    } catch (Exception $e) {
        throw new Exception('Error en la reserva: ' . $e->getMessage());
    }
}
?>

