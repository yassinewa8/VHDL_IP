library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_ARITH.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;

entity DualClockFIFO is
    Port (
        write_clk : in  STD_LOGIC;
        read_clk  : in  STD_LOGIC;
        reset     : in  STD_LOGIC; -- Synchronous reset
        write_en  : in  STD_LOGIC;
        read_en   : in  STD_LOGIC;
        data_in   : in  STD_LOGIC_VECTOR(7 downto 0);
        data_out  : out STD_LOGIC_VECTOR(7 downto 0);
        empty     : out STD_LOGIC;
        full      : out STD_LOGIC
    );
end DualClockFIFO;

architecture Behavioral of DualClockFIFO is
    type fifo_memory is array (0 to 3) of STD_LOGIC_VECTOR(7 downto 0);
    signal memory : fifo_memory;
    signal write_ptr_w : integer range 0 to 3 := 0;
    signal read_ptr_w : integer range 0 to 3 := 0;
    signal write_ptr_r : integer range 0 to 3 := 0;
    signal read_ptr_r : integer range 0 to 3 := 0;
    signal count : integer range 0 to 4 := 0;
begin

    process(write_clk, reset)
    begin
        if rising_edge(write_clk) then
            if reset = '1' then
                write_ptr_w <= 0;
            else
                if write_en = '1' and count < 4 then
                    memory(write_ptr_w) <= data_in;
                    write_ptr_w <= write_ptr_w + 1;
                    count <= count + 1;
                end if;
            end if;
        end if;
    end process;

    process(read_clk, reset)
    begin
        if rising_edge(read_clk) then
            if reset = '1' then
                read_ptr_r <= 0;
            else
                if read_en = '1' and count > 0 then
                    data_out <= memory(read_ptr_r);
                    read_ptr_r <= read_ptr_r + 1;
                    count <= count - 1;
                end if;
            end if;
        end if;
    end process;

    empty <= '1' when count = 0 else '0';
    full <= '1' when count = 4 else '0';

end Behavioral;
