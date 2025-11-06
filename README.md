# SPRSUN-CGK-V3L-B-Elfin-EW11

Remember ! Back up the original EW11 settings.

This topic concerns the integration of a Sprsun heat pump with a color display. Communication is via the Elfin EW11 module - settings: EW11_1,2.jpg.
![EW11_1](https://github.com/user-attachments/assets/fb88278b-fce9-4154-b150-79643692ef25)
![EW11_2](https://github.com/user-attachments/assets/5c5263c0-a21b-48f9-b817-fb2684b69154)
File .yaml [pompa_ciepla.yaml](https://github.com/user-attachments/files/23344865/pompa_ciepla.yaml)
It uses appropriate automations and helpers to write values ​​to the appropriate registers. The .yaml file with the automations:
[automatyzacja_pompa_ciepla.yaml](https://github.com/user-attachments/files/23396303/automatyzacja_pompa_ciepla.yaml)
# automatyzacje do PC

automation:

- id: sprsun_pc_wybieramy_tryb_pracy_pompy_obiegowej_19e
  alias: PC - wybór trybu pracy pompy obiegowej
  trigger:
    platform: state
    entity_id: input_select.sprsun_pc_g02_pump_work_0_interval_1_normal_2_demand_19e
    id: select
    to:
  condition: []
  action:
  - choose:
    - conditions:
      - condition: trigger
        id: input
      sequence:
      - variables:
          values:
            '0': 0 Interval
            '1': 1 Normal
            '2': 2 Demand
      - service: input_select.select_option
        target:
          entity_id: input_select.sprsun_pc_g02_pump_work_0_interval_1_normal_2_demand_19e
        data:
          option: '{{ values.get(trigger.to_state.state) }}'
    - conditions:
      - condition: trigger
        id: select
      sequence:
      - service: modbus.write_register
        data_template:
          address: 414
          slave: 1
          hub: Sprsun PC
          value: '{{ trigger.to_state.state[0] | int(0)  }}'
- id: sprsun_pc_wybieramy_tryb_pracy_kompresora_190
  alias: PC - wybór trybu pracy kompresora
  trigger:
    platform: state
    entity_id: input_select.sprsun_pc_tryb_pracy_kompresora_190
    id: select
    to:
  condition: []
  action:
  - choose:
    - conditions:
      - condition: trigger
        id: input
      sequence:
      - variables:
          values:
            '0': 0 Normal
            '1': 1 ECO
            '2': 2 Night
            '3': 3 Test
      - service: input_select.select_option
        target:
          entity_id: input_select.sprsun_pc_tryb_pracy_kompresora_190
        data:
          option: '{{ values.get(trigger.to_state.state) }}'
    - conditions:
      - condition: trigger
        id: select
      sequence:
      - service: modbus.write_register
        data_template:
          address: 400
          slave: 1
          hub: Sprsun PC
          value: '{{ trigger.to_state.state[0] | int(0)  }}'

- id: sprsun_pc_wybieramy_tryb_pracy_p06_unit_mode_36
  alias: PC - wybór trybu pracy PC
  trigger:
    platform: state
    entity_id: input_select.sprsun_pc_tryb_pracy_p06_unit_mode_36
    id: select
    to:
  condition: []
  action:
  - choose:
    - conditions:
      - condition: trigger
        id: input
      sequence:
      - variables:
          values:
            '0': 0 CWU
            '1': 1 Ogrzewanie
            '2': 2 Chłodzenie
            '3': 3 Ogrzewanie i CWU
            '4': 4 Chłodzenie i CWU
      - service: input_select.select_option
        target:
          entity_id: input_select.sprsun_pc_tryb_pracy_p06_unit_mode_36
        data:
          option: '{{ values.get(trigger.to_state.state) }}'
    - conditions:
      - condition: trigger
        id: select
      sequence:
      - service: modbus.write_register
        data_template:
          address: 54
          slave: 1
          hub: Sprsun PC
          value: '{{ trigger.to_state.state[0] | int(0)  }}'

- id: sprsun_pc_nastawiony_czas_po_g03_start_internal_185
  alias: PC - nastawa przerw w załączaniu pompy obiegowej
  trigger:
    platform: state
    entity_id: input_number.sprsun_pc_nastawiony_czas_po_g03_start_internal_185
  action:
    service: modbus.write_register
    data_template:
      address: 389
      hub: Sprsun PC
      unit: 1
      value: '{{ ((states(''input_number.sprsun_pc_nastawiony_czas_po_g03_start_internal_185'')
        | float) | multiply(1) | int) }}'

- id: sprsun_pc_nastawiony_czas_h08_defros_period_1_0038
  alias: PC - nastawa okresu miedzy odszranianiami 1
  trigger:
    platform: state
    entity_id: input_number.sprsun_pc_nastawiony_czas_h08_defros_period_1_0038
  action:
    service: modbus.write_register
    data_template:
      address: 56
      hub: Sprsun PC
      unit: 1
      value: '{{ ((states(''input_number.sprsun_pc_nastawiony_czas_h08_defros_period_1_0038'')
        | float) | multiply(1) | int) }}'

- id: sprsun_pc_nastawiony_czas_h11_defros_period_2_0039
  alias: PC - Nastawa okresu miedzy odszranianiami 2
  trigger:
    platform: state
    entity_id: input_number.sprsun_pc_nastawiony_czas_h11_defros_period_2_0039
  action:
    service: modbus.write_register
    data_template:
      address: 57
      hub: Sprsun PC
      unit: 1
      value: '{{ ((states(''input_number.sprsun_pc_nastawiony_czas_h11_defros_period_2_0039'')
        | float) | multiply(1) | int) }}'

- id: sprsun_pc_nastawiony_czas_h14_defros_period_3_003a
  alias: PC - Nastawa okresu miedzy odszranianiami 3
  trigger:
    platform: state
    entity_id: input_number.sprsun_pc_nastawiony_czas_h14_defros_period_3_003a
  action:
    service: modbus.write_register
    data_template:
      address: 58
      hub: Sprsun PC
      unit: 1
      value: '{{ ((states(''input_number.sprsun_pc_nastawiony_czas_h14_defros_period_3_003a'')
        | float) | multiply(1) | int) }}'

- id: sprsun_pc_nastawiona_max_freq_of_water_r00
  alias: PC - Nastawa max freq kompresora przy grzaniu CWU T>14
  trigger:
    platform: state
    entity_id: input_number.sprsun_pc_nastawiona_freq_of_water_r00
  action:
    service: modbus.write_register
    data_template:
      address: 252
      hub: Sprsun PC
      unit: 1
      value: '{{ ((states(''input_number.sprsun_pc_nastawiona_freq_of_water_r00'')
        | float) | multiply(1) | int) }}'

- id: sprsun_pc_nastawiona_max_freq_of_heat_r04
  alias: PC - Nastawa max freq kompresora przy ogrzewaniu T>14
  trigger:
    platform: state
    entity_id: input_number.sprsun_pc_nastawiona_freq_of_heat_r04
  action:
    service: modbus.write_register
    data_template:
      address: 260
      hub: Sprsun PC
      unit: 1
      value: '{{ ((states(''input_number.sprsun_pc_nastawiona_freq_of_heat_r04'')
        | float) | multiply(1) | int) }}'

- id: sprsun_pc_nastawiona_max_freq_of_water_r01
  alias: PC - Nastawa max freq kompresora przy grzaniu CWU 9>T<14
  trigger:
    platform: state
    entity_id: input_number.sprsun_pc_nastawiona_freq_of_water_r01
  action:
    service: modbus.write_register
    data_template:
      address: 253
      hub: Sprsun PC
      unit: 1
      value: '{{ ((states(''input_number.sprsun_pc_nastawiona_freq_of_water_r01'')
        | float) | multiply(1) | int) }}'

- id: sprsun_pc_nastawiona_max_freq_of_heat_r05
  alias: PC - Nastawa max freq kompresora przy ogrzewaniu 9>T<14
  trigger:
    platform: state
    entity_id: input_number.sprsun_pc_nastawiona_freq_of_heat_r05
  action:
    service: modbus.write_register
    data_template:
      address: 261
      hub: Sprsun PC
      unit: 1
      value: '{{ ((states(''input_number.sprsun_pc_nastawiona_freq_of_heat_r05'')
        | float) | multiply(1) | int) }}'

- id: sprsun_pc_nastawiona_min_freq_dc_pump
  alias: PC - Nastawa min freq pompy obiegowej
  trigger:
    platform: state
    entity_id: input_number.sprsun_pc_nastawiona_min_freq_dc_pump
  action:
    service: modbus.write_register
    data_template:
      address: 394
      hub: Sprsun PC
      unit: 1
      value: '{{ ((states(''input_number.sprsun_pc_nastawiona_min_freq_dc_pump'')
        | float) | multiply(1) | int) }}'

