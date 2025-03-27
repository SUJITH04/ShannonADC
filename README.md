### EXP NO 6: SHANNAON FANO CODING
### DATE:
### AIM : 
 To implement QPSK using MATLAB
 ### PROGRAM:
clc;
clear;
close all;

% Define symbols and their probabilities
symbols = {'A', 'B', 'C', 'D', 'E'};
probabilities = [0.4, 0.2, 0.2, 0.1, 0.1]; % Example probabilities

% Sort symbols based on probability (descending order)
[probabilities, sortIdx] = sort(probabilities, 'descend');
symbols = symbols(sortIdx);

% Generate Shannon-Fano Codes
codes = cell(size(symbols));
codes = shannon_fano_recursive(symbols, probabilities, codes, '');

% Display the Shannon-Fano codes
disp('Shannon-Fano Codes:');
for i = 1:length(symbols)
    fprintf('%s: %s\n', symbols{i}, codes{i});
end

% Example message to encode
message = {'A', 'C', 'B', 'A', 'E', 'D', 'B', 'A'};
encoded_message = encode_message(message, symbols, codes);
disp(['Encoded Message: ', encoded_message]);

% Decode the message
decoded_message = decode_message(encoded_message, symbols, codes);
disp('Decoded Message:');
disp(decoded_message);

%% Shannon-Fano Recursive Function
function codes = shannon_fano_recursive(symbols, probabilities, codes, prefix)
    if length(symbols) == 1
        codes{1} = prefix;
        return;
    end

    % Find the partition point where probability is split nearly in half
    total_prob = sum(probabilities);
    cumulative_prob = 0;
    split_index = 0;
    
    for i = 1:length(probabilities)
        cumulative_prob = cumulative_prob + probabilities(i);
        if cumulative_prob >= total_prob / 2
            split_index = i;
            break;
        end
    end

    % Recursively assign codes
    codes(1:split_index) = shannon_fano_recursive(symbols(1:split_index), ...
        probabilities(1:split_index), codes(1:split_index), strcat(prefix, '0'));
    codes(split_index+1:end) = shannon_fano_recursive(symbols(split_index+1:end), ...
        probabilities(split_index+1:end), codes(split_index+1:end), strcat(prefix, '1'));
end

%% Encoding Function
function encoded_msg = encode_message(message, symbols, codes)
    encoded_msg = '';
    for i = 1:length(message)
        index = find(strcmp(symbols, message{i}));
        encoded_msg = strcat(encoded_msg, codes{index});
    end
end

%% Decoding Function
function decoded_msg = decode_message(encoded_msg, symbols, codes)
    decoded_msg = {};
    temp = '';
    
    while ~isempty(encoded_msg)
        temp = strcat(temp, encoded_msg(1));
        encoded_msg(1) = [];

        index = find(strcmp(codes, temp), 1);
        if ~isempty(index)
            decoded_msg{end+1} = symbols{index}; %#ok<AGROW>
            temp = '';
        end
    end
end
### OUTPUT
Enter number of symbols    : 5
Enter symbol 1 : A
Enter symbol 2 : B
Enter symbol 3 : C
Enter symbol 4 : D
Enter symbol 5 : E

Enter probability of A : 0.22

Enter probability of B : 0.28

Enter probability of C : 0.15

Enter probability of D : 0.3

Enter probability of E : 0.05



    Symbol    Probability    Code
    D        0.3    00
    B        0.28    01
    A        0.22    10
    C        0.15    110
    E        0.05    111
### RESULT
Thus the generation of Shannon fano was implemented using MATLAB
