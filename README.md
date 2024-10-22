using System;
using System.Collections.Generic;

namespace HomeRepairApp
{
    // Класс для представления задачи по ремонту
    public class RepairTask
    {
        public int Id { get; set; }
        public string Description { get; set; }
        public bool IsCompleted { get; set; }
        public string Room { get; set; }
        public decimal EstimatedCost { get; set; }
        public DateTime DueDate { get; set; }
        public string Contractor { get; set; }

        // Конструктор
        public RepairTask(int id, string description, bool isCompleted, string room, 
                          decimal estimatedCost, DateTime dueDate, string contractor)
        {
            Id = id;
            Description = description;
            IsCompleted = isCompleted;
            Room = room;
            EstimatedCost = estimatedCost;
            DueDate = dueDate;
            Contractor = contractor;
        }
    }

    // Класс для управления списком задач по ремонту
    public class RepairManager
    {
        private List<RepairTask> tasks = new List<RepairTask>();
        private int nextTaskId = 1;
        private decimal totalCost = 0;

        // Добавление задачи
        public void AddTask(string description, string room, decimal estimatedCost, 
                           DateTime dueDate, string contractor = "")
        {
            RepairTask task = new RepairTask(nextTaskId++, description, false, room, 
                                          estimatedCost, dueDate, contractor);
            tasks.Add(task);
            totalCost += estimatedCost;
        }

        // Получение списка задач
        public List<RepairTask> GetTasks()
        {
            return tasks;
        }

        // Отметка задачи как выполненной
        public void MarkTaskCompleted(int id)
        {
            RepairTask task = tasks.Find(t => t.Id == id);
            if (task != null)
            {
                task.IsCompleted = true;
            }
        }

        // Удаление задачи
        public void RemoveTask(int id)
        {
            RepairTask task = tasks.Find(t => t.Id == id);
            if (task != null)
            {
                tasks.Remove(task);
                totalCost -= task.EstimatedCost;
            }
        }

        // Получение общей стоимости ремонта
        public decimal GetTotalCost()
        {
            return totalCost;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            // Создание менеджера задач по ремонту
            RepairManager repairManager = new RepairManager();

            // Добавление задач
            repairManager.AddTask("Покраска стен", "Гостиная", 1000, DateTime.Now.AddDays(7));
            repairManager.AddTask("Замена сантехники", "Ванная", 500, DateTime.Now.AddDays(14));
            repairManager.AddTask("Установка новой плитки", "Кухня", 1500, DateTime.Now.AddDays(21), "Иван Иванов");

            // Вывод списка задач
            Console.WriteLine("Список задач по ремонту:");
            foreach (RepairTask task in repairManager.GetTasks())
            {
                Console.WriteLine($"{task.Id}. {task.Description} ({task.Room}) - Стоимость: {task.EstimatedCost:C} (Срок: {task.DueDate:dd.MM.yyyy}) {(task.Contractor != "" ? $" - Подрядчик: {task.Contractor}" : "")} {(task.IsCompleted ? "(Выполнено)" : "")}");
            }

            Console.WriteLine($"\nОбщая стоимость ремонта: {repairManager.GetTotalCost():C}");

            // Отметка задачи как выполненной
            repairManager.MarkTaskCompleted(1);
            Console.WriteLine("\nЗадача с Id 1 отмечена как выполненная.");

            // Удаление задачи
            repairManager.RemoveTask(2);
            Console.WriteLine("\nЗадача с Id 2 удалена.");

            // Вывод обновленного списка задач
            Console.WriteLine("\nОбновленный список задач:");
            foreach (RepairTask task in repairManager.GetTasks())
            {
                Console.WriteLine($"{task.Id}. {task.Description} ({task.Room}) - Стоимость: {task.EstimatedCost:C} (Срок: {task.DueDate:dd.MM.yyyy}) {(task.Contractor != "" ? $" - Подрядчик: {task.Contractor}" : "")} {(task.IsCompleted ? "(Выполнено)" : "")}");
            }

            Console.WriteLine($"\nОбщая стоимость ремонта: {repairManager.GetTotalCost():C}");

            Console.ReadKey();
        }
    }
}
