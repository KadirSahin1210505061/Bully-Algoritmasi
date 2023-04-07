# Bully-Algoritmasi
Bully algoritması, bir dağıtık sistemde lider seçimi için kullanılan bir algoritmadır.


using System;
using System.Collections.Generic;

class BullyAlgorithm
{
    static void Main(string[] args)
    {
        int n = 5; // number of processes in the distributed system
        int coordinator = n; // initialize coordinator as the last process

        // create list of processes with their respective IDs
        List<int> processes = new List<int>();
        for (int i = 1; i <= n; i++)
        {
            processes.Add(i);
        }

        // simulate a process failure
        int failedProcess = 3;
        processes.Remove(failedProcess);

        // simulate an election
        foreach (int process in processes)
        {
            if (process > coordinator)
            {
                Console.WriteLine("Process {0} started an election.", process);
                if (sendElection(process))
                {
                    coordinator = process;
                }
            }
        }

        Console.WriteLine("Process {0} is the new coordinator.", coordinator);
    }

    static bool sendElection(int process)
    {
        // send election message to higher priority processes
        bool higherPriorityResponded = false;
        for (int i = process + 1; i <= n; i++)
        {
            Console.WriteLine("Process {0} sent an election message to process {1}.", process, i);
            // wait for response from higher priority processes
            if (responds(process, i))
            {
                higherPriorityResponded = true;
                break;
            }
        }

        // if no higher priority processes respond, declare as coordinator
        if (!higherPriorityResponded)
        {
            Console.WriteLine("Process {0} is the new coordinator.", process);
            return true;
        }
        else
        {
            return false;
        }
    }

    static bool responds(int sender, int receiver)
    {
        // simulate a response from a process
        Random random = new Random();
        int response = random.Next(2);
        if (response == 1)
        {
            Console.WriteLine("Process {0} responded to process {1}.", receiver, sender);
            return true;
        }
        else
        {
            Console.WriteLine("Process {0} did not respond to process {1}.", receiver, sender);
            return false;
        }
    }
}


Algoritmaların Amacı ve Çalışma Şekli:
Bully algoritması, dağıtık sistemlerde lider seçimi için kullanılır. Sistemdeki her bir süreç birbirleriyle haberleşerek lider seçimini gerçekleştirir. Bully algoritması çalışma şekli olarak şu adımları içerir:

Bir süreç lider olup olmadığını kontrol eder. Lider olduğunu düşünürse diğer süreçlere lider olduğunu bildirir.
Bir süreç lider olmadığını düşünürse, diğer süreçlere lider seçimi başlatır.
Lider seçimi başlatıldığında, süreçler öncelik sırasına göre diğer süreçlere liderlik talebinde bulunurlar. Öncelikli süreçler, diğer süreçlerden yanıt alana kadar beklerler.
4. Süreçler, liderlik talebine yanıt vermezlerse, daha yüksek önceliğe sahip süreçlere liderlik talebi gönderirler.

En yüksek önceliğe sahip süreç liderlik talebine yanıt verirse, lider seçimi sona erer ve bu süreç lider olur.

Eğer en yüksek önceliğe sahip süreç liderlik talebine yanıt vermezse, diğer süreçler lider seçimini tekrar başlatır.

Algoritmaların Çalışma Zamanı Analizi:

Bully algoritması, en kötü durumda O(n^2) zaman karmaşıklığına sahiptir. En iyi durumda ise O(nlogn) zaman karmaşıklığına sahiptir. Ortalama durumda ise, O(nlogn) zaman karmaşıklığı beklenir.

En kötü durum, liderlik pozisyonunun sürekli değiştiği durumdur. Bu durumda her süreç, diğer süreçlere liderlik talebinde bulunacaktır. Bu nedenle, tüm süreçlerin önceliklerine göre lider seçimi yapılması için n^2 işlem yapılması gerekebilir.

En iyi durum, liderlik pozisyonunun sabit olduğu durumdur. Bu durumda, liderlik talepleri sırayla gönderilir ve lider seçimi hızlı bir şekilde tamamlanır. Bu durumda, n süreç için logn kadar işlem yapılması beklenir.

Ortalama durumda, liderlik pozisyonunun değişim sıklığına ve dağıtık sistemdeki süreç sayısına bağlı olarak lider seçimi için logn kadar işlem yapılması beklenir.

Bu analiz, lider seçimi için yapılan işlemlerin sayısını hesaba katarak yapılmıştır. Ancak, Bully algoritmasındaki işlemlerin tamamlanması için gereken süre, süreçlerin birbirlerine mesaj gönderme hızlarına, ağdaki gecikmelere ve süreçlerin donanımsal kapasitelerine de bağlıdır.
