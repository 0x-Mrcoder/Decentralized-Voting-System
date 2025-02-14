This is a decentralized voting system built with Solidity. It allows users to create polls, vote for candidates, and view the results transparently on the blockchain.
Features:
Create polls with multiple candidates.
Vote for a candidate once per address.
End polls and view results.

Smart Contract Overview:
CreatePoll: Creates a new poll with candidates and a title.
Voting: Allows users to vote for a candidate in the current poll.
EndVote: Ends the current poll, preventing further votes.
ShowResult: Displays the result of a poll for a specific candidate.

Example of creating a poll
CreatePoll(["Alice", "Bob", "Charlie"], "Class Representative Election");









// SPDX-License-Identifier: MIT
// Mr Coder
pragma solidity ^0.8.16;

contract VotingSystem {
    struct Poll {
        string[] Candidates;
        string Title;
        mapping(address => bool) IsVoted;
        mapping(uint256 => uint256) Votes;
        bool Ended;
    }
    
    uint256 public Count;
    mapping (uint => Poll) public polls;

    function CreatePoll(string[] memory _Candidates, string memory _Title) public {
       Poll storage newPoll = polls[Count];
       newPoll.Title = _Title;
       newPoll.Candidates = _Candidates;
       newPoll.Ended = false;
       Count++;
    }

    function Voting(uint256 _Candidate) public  {
       Poll storage currentPoll = polls[Count - 1];

       require(!currentPoll.Ended, "Poll has ended");
       require(!currentPoll.IsVoted[msg.sender], "You already voted");
       require(_Candidate < currentPoll.Candidates.length, "Invalid candidate");
       
       currentPoll.Votes[_Candidate]++;
       currentPoll.IsVoted[msg.sender] = true;
    }


    function EndVote(bool _Ended) public {
        polls[Count - 1].Ended = _Ended;
    }

    function ShowResult(uint256 _pollId, uint256 _Candidate) public view returns (uint256) {
       return polls[_pollId].Votes[_Candidate];
    }

    function GetTitle(uint256 _pollId) public view returns (string memory) {
        return polls[_pollId].Title;
    }


    function GetCandidates(uint256 _pollId) public view returns (string[] memory) {
        return polls[_pollId].Candidates;
    }
}


Edit
Developed by Usman Umar (Mr Coder)
