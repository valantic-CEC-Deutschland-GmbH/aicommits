#!/usr/bin/php
<?php

function runCommand($command): string {
    exec($command, $output, $returnCode);
    if ($returnCode !== 0) {
        throw new Exception("Command $command failed with exit code $returnCode");
    }
    return implode("\n", $output);
}

function checkIfCommitsAreStaged(): bool {
    try {
        $result = runCommand('git diff --staged');
        if ($result === '') {
            return false;
        }
    } catch (Exception $e) {
        return false;
    }
    return true;
}

function generateCommitMessageFromDiff($diff): string {
    $prompt = "Given the following git patch file:\n$diff\n###\nGenerate a one-sentence long git commit message.\nReturn only the commit message without comments or other text.";

    $openAiKey = getenv('OPENAI_KEY');
    $endpoint = 'https://api.openai.com/v1/completions';

    $data = [
        'prompt' => $prompt,
        'temperature' => 0,
        'model' => 'text-davinci-003',
        'max_tokens' => 128
    ];

    $headers = [
        'Content-Type: application/json',
        'Authorization: Bearer ' . $openAiKey
    ];

    $ch = curl_init($endpoint);
    curl_setopt($ch, CURLOPT_POST, 1);
    curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($data));
    curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

    $response = curl_exec($ch);
    curl_close($ch);

    $responseData = json_decode($response, true);
    $message = $responseData['choices'][0]['text'];
    return str_replace(['"', "\n"], '', $message);
}

function addBranchToCommitMessage(string $commitMsg): string {
    $branchName = runCommand('git rev-parse --abbrev-ref HEAD');

    if (in_array($branchName, ['staging', 'main', 'master']) || str_starts_with($branchName, 'no-task')) {
        $branchName = 'no-task';
    } elseif (str_contains($branchName, '/')) {
        $branchName = explode('/', $branchName)[1];
    }
    return $branchName . ' ' . $commitMsg;
}

if (basename(__FILE__) === basename($_SERVER['PHP_SELF'])) {
    if (!checkIfCommitsAreStaged()) {
        echo 'No staged commits';
        exit(0);
    }
    $diff = runCommand('git diff --staged');
    $commitMessage = generateCommitMessageFromDiff($diff);
    $commitMessage = addBranchToCommitMessage($commitMessage);

    echo json_encode($commitMessage);
}
