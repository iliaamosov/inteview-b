Описание
Эта роль настраивает энергосбережение и производительность CPU на сервере.


Выполняются следующие задачи:
Отключение C-state (энергосбережения процессора).
Переключение CPU в режим производительности (performance mode).
Настройка Intel P-State (если поддерживается).


1. Проверка текущего состояния C-state

Выполняем команду: cat /sys/module/intel_idle/parameters/max_cstate и сохраняем результат.

2. Отключение C-state (энергосбережения процессора)
Задача:
Если C-state > 0, отключаем его.

Устанавливаем max_cstate = 0:
echo 0 | sudo tee /sys/module/intel_idle/parameters/max_cstate

Выполняется, если current_cstate.stdout | int > 0.


3. Проверка включенного энергосбережения
 
Выполняем команду:
cat /sys/devices/system/cpu/cpu*/cpuidle/state*/disable
Если есть 0, значит C-state активен.

4. Отключение C-state на всех ядрах

echo 1 | sudo tee /sys/devices/system/cpu/cpu*/cpuidle/state*/disable

Выполняется, Если в предыдущей проверке найден 0.


5. Проверка доступности CPUFreq

Проверяем, существует ли файл: /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor
Если да, значит CPUFreq можно настроить.

6. Переключение CPU в режим высокой производительности

echo performance | sudo tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
Выполняется, если cpufreq_check.stat.exists == true.

7. Проверка поддержки Intel P-State

Проверяем, существует ли файл /sys/devices/system/cpu/intel_pstate/status
Если да, значит Intel P-State поддерживается.

8. Установка максимальной производительности через Intel PState

echo performance | sudo tee /sys/devices/system/cpu/intel_pstate/max_perf_pct

Когда выполняется:
Если intel_pstate_check.stat.exists == true.





Запустите роли:


ansible-playbook -i inventory/hosts.ini playbook.yml --tags cpu_update
ansible-playbook -i inventory/hosts.ini playbook.yml --tags cpu_update --check


