# mipz-refactor


Варіант 2

Мороз Михайло Володимирович КП-11мп

```

// task1
void checkSecurity(String[] people)
{
 boolean found = false;
 for (int i=0; i < people.Length; i++)
 {
   if (!found)
   {
      if (people[i].Equals (“Don”))
      {
         sendAlert();
         found = true;
      }
      // запах забруднювачі, дублювання коду
      if (people[i].Equals (“John”))
      {
         sendAlert();
         found = true;
      }
    }
  }
}

// запах - роздвувальщики
// використовується застарілий процедурний варіант цилку
// замість цього можна застосувати більш функціональний підхід
// судячи з контексту, константи імен це sensetive дані, які можна скомпроментувати
// треба їх перенести в якесь більш секьюрне середовище, типу Keychain з шифруванням

void checkSecurity(String[] people)
{
  String[] whitelistedNames = NameKeychain.getWhitelistedNames();
  boolean found = people.Any(
    person => whitelistedNames.Contains(person)
  );
  if (found) { sendAlert(); }
}


// task2
enum TransportType
{
  eCar,
  ePlane,
  eSubmarine
};
 
class Transport
{
public:
 Transport(TransportType type) : m_type (type) {}
 
 int GetSpeed(int distance, int time)
 {
   if (time != 0)
   {
      switch(m_type)
      {    
      case eCar:
        return distance/time;
      case ePlane:
        return  distance/(time - getTakeOffTime() - getLandingTime());
      case eSubmarine:
        return distance/(time - getDiveTime() - getAscentTime());
      }
   }
 }

private:
  int m_takeOffTime;
  int m_landingTime;
  int m_diveTime;
  int m_ascentTime;
  TransportType m_type;
};

// запах порушиники ООП
// замість використання enum і switch можна делегувати GetSpeed по різних классах
// також використовуючи різні класси можна рознести потрібні поля, а не группувати всі поля в одному классі
// використати можна абстрактний класс або інтерфейс, залежачи від контексту

public abstract class Transport {
  public:
    Transport() {
      // ...
    }
  abstract GetSpeed(int distance, int time);

}

public class Car : Transport {
  public:
    override int GetSpeed(int distance, int time) {
      return distance / time;
    }

  // ...
}

public class Plane : Transport {
  private:
    int m_takeOffTime;
    int m_landingTime;

  public:
    override int GetSpeed(int distance, int time) {
      return distance/(time - getTakeOffTime() - getLandingTime());
    }
  // ...
}

public class Submarine : Transport {
  private:
     int m_diveTime;
     int m_ascentTime;

  public:
    override int GetSpeed(int distance, int time) {
      return distance / (time - getDiveTime() - getAscentTime());
    }
  // ...
}
```
