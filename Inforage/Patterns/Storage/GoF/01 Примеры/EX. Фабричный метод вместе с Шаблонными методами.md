Представим систему, которая генерирует отчеты в различных форматах (например, PDF и Excel). Мы будем использовать **Шаблонный метод** для определения общего процесса создания отчета и **Фабричный метод** для создания конкретных форматов отчета.

Создадим абстрактный класс `Report`, который определяет общий алгоритм для создания отчета. В этом классе мы будем использовать фабричный метод для создания конкретного формата отчета.

```java
public abstract class Report {
    // Шаблонный метод
    public final void createReport() {
        gatherData();
        generateReport();
        saveReport();
    }

    // Шаги, которые могут быть изменены
    protected abstract void gatherData();
    protected abstract void generateReport();
    
    // Шаги, которые не изменяются
    private void saveReport() {
        ReportFormat format = createReportFormat();
        format.save();
    }

    // Фабричный метод для создания формата отчета
    protected abstract ReportFormat createReportFormat();
}
```

Создадим интерфейс `ReportFormat` для определения формата отчета и его сохранения.

```java
public interface ReportFormat {
    void save();
}
```

Реализуем конкретные форматы отчета, такие как PDF и Excel.

```java
public class PDFReportFormat implements ReportFormat {
    @Override
    public void save() {
        System.out.println("Saving report as PDF");
    }
}

public class ExcelReportFormat implements ReportFormat {
    @Override
    public void save() {
        System.out.println("Saving report as Excel");
    }
}
```

Реализуем конкретные классы отчетов, которые определяют, как собирать данные и генерировать отчет, а также используют фабричный метод для создания формата отчета.

```java
public class PDFReport extends Report {
    @Override
    protected void gatherData() {
        System.out.println("Gathering data for PDF report");
    }

    @Override
    protected void generateReport() {
        System.out.println("Generating PDF report");
    }

    @Override
    protected ReportFormat createReportFormat() {
        return new PDFReportFormat();
    }
}

public class ExcelReport extends Report {
    @Override
    protected void gatherData() {
        System.out.println("Gathering data for Excel report");
    }

    @Override
    protected void generateReport() {
        System.out.println("Generating Excel report");
    }

    @Override
    protected ReportFormat createReportFormat() {
        return new ExcelReportFormat();
    }
}
```

Клиентский код создает различные отчеты, используя абстрактный класс и фабричный метод для создания формата отчета.

```java
public class ReportDemo {
    public static void main(String[] args) {
        Report pdfReport = new PDFReport();
        pdfReport.createReport();
        
        System.out.println();
        
        Report excelReport = new ExcelReport();
        excelReport.createReport();
    }
}
```

- **Шаблонный метод** (`createReport()`) в классе `Report` определяет общий процесс создания отчета, включая шаги для сбора данных, генерации отчета и сохранения его.
- **Фабричный метод** (`createReportFormat()`) в классе `Report` позволяет создать конкретный формат отчета (например, PDF или Excel).
- **Конкретные отчеты** (`PDFReport`, `ExcelReport`) реализуют методы для сбора данных и генерации отчета, а также используют фабричный метод для создания формата отчета.
- **Форматы отчета** (`PDFReportFormat`, `ExcelReportFormat`) реализуют интерфейс `ReportFormat` и определяют, как сохранять отчет в конкретном формате.