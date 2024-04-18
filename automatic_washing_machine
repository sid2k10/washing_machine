`timescale 1ns / 1ps


module automatic_washing_machine(
    input clk, reset, door_close, start, filled, detergent_added, cycle_timeout, drained, spin_timeout,
    output reg door_lock, motor_on, fill_value_on, drain_value_on, done, soap_wash, water_wash
);

    //defining the states
    parameter check_door = 3'b000;
    parameter fill_water = 3'b001;
    parameter add_detergent = 3'b010;
    parameter cycle = 3'b011;
    parameter drain_water = 3'b100;
    parameter spin = 3'b101;
    
    reg[2:0] current_state, next_state;
    
    // State Transition Logic
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            current_state <= check_door;
        end else begin
            current_state <= next_state;
        end
    end

    // Next State Logic
    always @(current_state or start or door_close or filled or detergent_added or drained or cycle_timeout or spin_timeout)
    begin
        case(current_state)
            check_door: begin
                if(start && door_close)
                    next_state = fill_water;
                else
                    next_state = check_door;
            end
            fill_water: begin
                if (filled) begin
                    if(soap_wash == 0)
                        next_state = add_detergent;
                    else
                        next_state = cycle;
                end else
                    next_state = fill_water;
            end
            add_detergent: begin
                if(detergent_added)
                    next_state = cycle;
                else
                    next_state = add_detergent;
            end
            cycle: begin
                if(cycle_timeout)
                    next_state = drain_water;
                else
                    next_state = cycle;
            end
            drain_water: begin
                if(drained) begin
                    if(water_wash==0)
                        next_state = fill_water;
                    else
                        next_state = spin;
                end else
                    next_state = drain_water;
            end
            spin: begin
                if(spin_timeout)
                    next_state = check_door;
                else
                    next_state = spin;
            end
            default: next_state = check_door;
        endcase
    end

    // Output Logic
    always @(current_state) begin
        case(current_state)
            check_door: begin
                door_lock = 0; motor_on = 0; fill_value_on = 0; drain_value_on = 0; done = 0; soap_wash = 0; water_wash = 0;
            end
            fill_water: begin
                door_lock = 1; motor_on = 0; fill_value_on = 1; drain_value_on = 0; done = 0; soap_wash = 1; water_wash = 0;
            end
            add_detergent: begin
                door_lock = 1; motor_on = 0; fill_value_on = 0; drain_value_on = 0; done = 0; soap_wash = 1; water_wash = 0;
            end
            cycle: begin
                door_lock = 1; motor_on = 1; fill_value_on = 0; drain_value_on = 0; done = 0; soap_wash = 1; water_wash = 1;
            end
            drain_water: begin
                door_lock = 1; motor_on = 0; fill_value_on = 0; drain_value_on = 1; done = 0; soap_wash = 1; water_wash = 1;
            end
            spin: begin
                door_lock = 1; motor_on = 1; fill_value_on = 0; drain_value_on = 0; done = 0; soap_wash = 1; water_wash = 1;
            end
            default: begin
                door_lock = 0; motor_on = 0; fill_value_on = 0; drain_value_on = 0; done = 0; soap_wash = 0; water_wash = 0;
            end
        endcase
    end

endmodule
