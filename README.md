// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SocialMedia {
    struct Profile {
        string username;
        string profileCID; // IPFS hash for profile image & bio
    }

    struct Post {
        uint256 id;
        address author;
        string contentCID; // IPFS hash of post content
        uint256 timestamp;
    }

    mapping(address => Profile) public profiles;
    mapping(uint256 => Post) public posts;
    uint256 public postCount;

    event ProfileUpdated(address indexed user, string username, string profileCID);
    event PostCreated(uint256 indexed postId, address indexed author, string contentCID);

    function setProfile(string memory _username, string memory _profileCID) public {
        profiles[msg.sender] = Profile(_username, _profileCID);
        emit ProfileUpdated(msg.sender, _username, _profileCID);
    }

    function createPost(string memory _contentCID) public {
        postCount++;
        posts[postCount] = Post(postCount, msg.sender, _contentCID, block.timestamp);
        emit PostCreated(postCount, msg.sender, _contentCID);
    }

    function getPost(uint256 _postId) public view returns (Post memory) {
        return posts[_postId];
    }

    function getProfile(address _user) public view returns (Profile memory) {
        return profiles[_user];
    }
}
