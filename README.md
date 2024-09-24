import java.util.Scanner;
import java.util.Random;

abstract class Weapon {
    public abstract void attack(Character opponent);
    public abstract void defense();
    public abstract int calculateDamage();

    // 랜덤 데미지 계산 메서드
    protected int calculateRandomDamage(int minDamage, int maxDamage) {
        Random rand = new Random();
        return rand.nextInt(maxDamage - minDamage + 1) + minDamage;
    }
}

class Character {
    private String name;
    private int hp;

    public Character(String name, int initialHp) {
        this.name = name;
        this.hp = initialHp;
    }

    public void attack(Weapon weapon, Character opponent) {
        weapon.attack(opponent);
    }

    public void takeDamage(int damage) {
        if (damage > 0) {
            this.hp -= damage;
            if (this.hp <= 0) {
                this.hp = 0;
                System.out.printf("%s이(가) 쓰러졌습니다!\n", this.name);
                System.out.println("게임을 종료합니다.");
                System.exit(0);
            } else {
                System.out.printf("%s이(가) %d의 데미지를 입었습니다. 남은 HP: %d\n", this.name, damage, this.hp);
            }
        } else {
            System.out.println("유효하지 않은 데미지입니다.");
        }
    }

    public int getHp() {
        return hp;
    }
}

class Bow extends Weapon {
    public Bow() {
        System.out.println("활을 선택하셨습니다");
    }

    @Override
    public void attack(Character opponent) {
        System.out.println("활로 상대방을 공격합니다");
        int damage = calculateDamage();
        opponent.takeDamage(damage);
    }

    @Override
    public void defense() {
        System.out.println("활로 방어할 수 없습니다");
    }

    @Override
    public int calculateDamage() {
        return calculateRandomDamage(7, 15);
    }
}

class Sword extends Weapon {
    public Sword() {
        System.out.println("검을 선택하셨습니다");
    }

    @Override
    public void attack(Character opponent) {
        System.out.println("검으로 상대방을 공격합니다");
        int damage = calculateDamage();
        opponent.takeDamage(damage);
    }

    @Override
    public void defense() {
        System.out.println("검으로 상대방의 공격을 막습니다");
    }

    @Override
    public int calculateDamage() {
        return calculateRandomDamage(3, 27);
    }
}

class Shield extends Weapon {
    public Shield() {
        System.out.println("방패를 선택하셨습니다");
    }

    @Override
    public void attack(Character opponent) {
        System.out.println("방패로 상대방을 방어합니다");
        defense();
    }

    @Override
    public void defense() {
        System.out.println("방패로 상대방의 공격을 무효화합니다");
    }

    @Override
    public int calculateDamage() {
        return 0; // 방패는 공격 데미지가 없습니다
    }
}

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Character player = new Character("플레이어", 100);
        Character opponent = new Character("상대방", 100);

        try {
            System.out.println("게임을 시작합니다.");
            Thread.sleep(1500);
            System.out.println("자동저장 중에 게임을 종료하지 마세요.");
            Thread.sleep(1500);
            System.out.println("과도한 게임 이용은 정신 건강에 해로울 수 있습니다.");
            Thread.sleep(1500);
            System.out.println(" ");

            while (player.getHp() > 0 && opponent.getHp() > 0) {
                System.out.println("무기를 선택하세요 (1 = 활, 2 = 검, 3 = 방패, 99 = 종료)");
                int choice = scanner.nextInt();

                switch (choice) {
                    case 1:
                        Bow bow = new Bow();
                        player.attack(bow, opponent);
                        break;
                    case 2:
                        Sword sword = new Sword();
                        player.attack(sword, opponent);
                        break;
                    case 3:
                        Shield shield = new Shield();
                        player.attack(shield, opponent);
                        break;
                    case 99:
                        System.out.println("프로그램을 종료합니다.");
                        break;
                    default:
                        System.out.println("잘못된 선택입니다. 다시 선택하세요.");
                        break;
                }

                if (opponent.getHp() > 0) {
                    // 상대방의 공격 (랜덤 무기 선택)
                    Weapon opponentWeapon = getRandomWeapon();
                    opponent.attack(opponentWeapon, player);
                }
            }

            scanner.close();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    // 랜덤 무기 선택 메서드
    private static Weapon getRandomWeapon() {
        Random rand = new Random();
        int choice = rand.nextInt(3) + 1; // 1부터 3까지 랜덤 선택a
        switch (choice) {
            case 1:
                return new Bow();
            case 2:
                return new Sword();
            case 3:
                return new Shield();
            default:
                return new Sword(); // 기본적으로 검 반환
        }
    }
}
