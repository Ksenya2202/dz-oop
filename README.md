class Container:
    def __init__(self, cargo_weight):
        self.weight = 3700           # вес ктк
        self.cargo = cargo_weight    # вес груза


class Truck:
    def __init__(self, container=None):
        self.container = container

    def total_weight(self):
        if self.container:
            return self.container.weight + self.container.cargo
        else:
            return 0

    def can_leave(self):
        if self.container:
            return self.total_weight() <= 30000
        return False


class Warehouse:
    def check_truck(self, truck):
        if truck.can_leave():
            print(f"Контейнеровоз может выехать. Общий вес: {truck.total_weight()} кг.")
        else:
            print(f"Контейнеровозу нельзя выехать. Общий вес: {truck.total_weight()} кг. ")


# ктк с грузом
container = Container(cargo_weight=26000) 

truck = Truck(container)

#выезд, въезд
warehouse = Warehouse()
warehouse.check_truck(truck)