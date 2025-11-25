from django.db import models

class Huesped(models.Model):
    id_huesped = models.AutoField(primary_key=True)
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    fecha_nacimiento = models.DateField()
    nacionalidad = models.CharField(max_length=50)
    direccion = models.CharField(max_length=255)
    telefono = models.CharField(max_length=20)
    email = models.CharField(max_length=100)
    dni_pasaporte = models.CharField(max_length=50)
    fecha_registro = models.DateField()

    def __str__(self):
        return f"{self.nombre} {self.apellido}"


class HabitacionHotel(models.Model):
    id_habitacion = models.AutoField(primary_key=True)
    numero_habitacion = models.CharField(max_length=10)
    tipo_habitacion = models.CharField(max_length=50)
    capacidad = models.IntegerField()
    precio_noche = models.DecimalField(max_digits=10, decimal_places=2)
    estado_habitacion = models.CharField(max_length=50)
    caracteristicas = models.TextField()
    piso = models.IntegerField()

    def __str__(self):
        return f"Habitación {self.numero_habitacion}"


class ReservaHotel(models.Model):
    id_reserva = models.AutoField(primary_key=True)
    id_huesped = models.ForeignKey(Huesped, on_delete=models.CASCADE)
    fecha_entrada = models.DateField()
    fecha_salida = models.DateField()
    numero_adultos = models.IntegerField()
    numero_ninos = models.IntegerField()
    estado_reserva = models.CharField(max_length=50)
    total_reserva = models.DecimalField(max_digits=10, decimal_places=2)
    fecha_reserva = models.DateTimeField()

    def __str__(self):
        return f"Reserva {self.id_reserva}"


class DetalleReserva(models.Model):
    id_detalle_reserva = models.AutoField(primary_key=True)
    id_reserva = models.ForeignKey(ReservaHotel, on_delete=models.CASCADE)
    id_habitacion = models.ForeignKey(HabitacionHotel, on_delete=models.CASCADE)
    precio_noche_reservado = models.DecimalField(max_digits=10, decimal_places=2)
    cantidad_noches = models.IntegerField()
    subtotal = models.DecimalField(max_digits=10, decimal_places=2)
    servicios_adicionales = models.TextField()

    def __str__(self):
        return f"Detalle {self.id_detalle_reserva}"


class EmpleadoHotel(models.Model):
    id_empleado = models.AutoField(primary_key=True)
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    cargo = models.CharField(max_length=50)
    fecha_contratacion = models.DateField()
    salario = models.DecimalField(max_digits=10, decimal_places=2)
    telefono = models.CharField(max_length=20)
    email = models.CharField(max_length=100)
    dni = models.CharField(max_length=20)

    def __str__(self):
        return f"{self.nombre} {self.apellido}"


class ServicioAdicional(models.Model):
    id_servicio = models.AutoField(primary_key=True)
    nombre_servicio = models.CharField(max_length=100)
    descripcion = models.TextField()
    costo_unitario = models.DecimalField(max_digits=10, decimal_places=2)
    disponible = models.BooleanField()
    categoria = models.CharField(max_length=50)
    duracion = models.CharField(max_length=50)

    def __str__(self):
        return self.nombre_servicio


class FacturaHotel(models.Model):
    id_factura = models.AutoField(primary_key=True)
    id_reserva = models.ForeignKey(ReservaHotel, on_delete=models.CASCADE)
    fecha_emision = models.DateField()
    total_factura = models.DecimalField(max_digits=10, decimal_places=2)
    estado_pago = models.CharField(max_length=50)
    metodo_pago = models.CharField(max_length=50)
    id_huesped_pago = models.ForeignKey(Huesped, on_delete=models.CASCADE)
    fecha_vencimiento = models.DateField()

    def __str__(self):
        return f"Factura {self.id_factura}"
