# potok
Программа исполняет вывод инфы и запись инфы потоками
import java.nio.file.*;
import java.util.*;
import java.io.*;

public class Num {
    public static void main(String...args) throws IOException, InterruptedException {
        String InfFromOut;
        Scanner in = new Scanner(System.in);

        System.out.println("Введите направление для считывания иноформации:");
        String FOut=in.nextLine();

        System.out.println("Введите направление для записи информации:");
        String FIn=in.nextLine();



        MyThreadOut Out=new MyThreadOut(FOut,"Считывает информацию ");
        InfFromOut=Out.InfOut();

        MyThreadIn In=new MyThreadIn(FIn,"Записывает информацию",InfFromOut);

    }
}


class MyThreadIn implements Runnable {
    Thread thrdIn;
    String name;
    String FName;
    String NeedIn;

    MyThreadIn(String FName, String name,String Information) throws InterruptedException {
        this.name = name;
        thrdIn = new Thread(this, name);
        this.NeedIn=Information;
        this.FName = FName;
        thrdIn.sleep(700);
        thrdIn.start();

    }

    @Override
    public void run() {


        if(thrdIn.isAlive()) {
            System.out.println("Поток для записи успешно запущен.");
            try (FileWriter writer = new FileWriter(FName, false)) {
                // запись всей строки
                writer.write(NeedIn);
                // запись по символам
System.out.println("Поток успешно записал в файл текст!");
            } catch (IOException ex) {
                System.out.println(ex.getMessage());
            }
        }else System.out.println("При записи в файл произошла внутреняя ошибка");
    }

}


class MyThreadOut  implements Runnable {
    Thread thrdOut;
    String name;
    String FName;
    String strOut;

    MyThreadOut(String FName, String name) {
        this.name = name;
        thrdOut = new Thread(this, name);
        this.FName = FName;
        thrdOut.start();
    }

    @Override
    public void run() {
        if (thrdOut.isAlive()) {
            System.out.println("Поток для получения инофрмации из файла успешно запущен");

            try {
                System.out.println("Считаный текст:" + InfOut());
            } catch (IOException e) {
                e.printStackTrace();
            }
        }else System.out.println("Произошла внутреняя ошибка ошибка");
    }

    public String InfOut()throws IOException{

        Path path = Paths.get(FName);

        BufferedReader reader = Files.newBufferedReader(path);
        strOut = reader.readLine();
        return strOut;
    }
}
