using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{


    Animator _playerAnimator;

    Transform _centerPointer;

    public float _speedMovement, _timeToMoveAnimation, _speedRotationAround, _angleOffset;

    public bool _forward, _back, _right, _left, _sprint, _jump;


    // Use this for initialization
    void Start()
    {
        _playerAnimator = GetComponent<Animator>();
        _centerPointer = GameObject.FindGameObjectWithTag("centerPointer").transform;

    }

    // Update is called once per frame
    void Update()
    {

        _forward = Input.GetAxis("Vertical") > 0.1f;
        _back = Input.GetAxis("Vertical") < -0.1f;
        _right = Input.GetAxis("Horizontal") > 0.1f;
        _left = Input.GetAxis("Horizontal") < -0.1f;

        _sprint = Input.GetButton("Sprint");
        _jump = Input.GetButton("Jump");

        Debug.Log(_sprint);

        Movement();
        CalulateAngle();
    }

    void Movement()
    {

        float _MoveValue = Mathf.Max(Mathf.Abs(Input.GetAxis("Horizontal")), Mathf.Abs(Input.GetAxis("Vertical")));


        _playerAnimator.SetFloat("Forward", _MoveValue * _speedMovement, _timeToMoveAnimation, Time.deltaTime);
        _playerAnimator.SetBool("Jump", _jump);

        if (_MoveValue > 0.1f)

        {
            _playerAnimator.SetBool("_Sprint01", _sprint);

            if (_jump)
            {
               
                _playerAnimator.SetTrigger("Jump");
            }

            transform.eulerAngles = new Vector3(0, Mathf.LerpAngle(transform.eulerAngles.y, _centerPointer.eulerAngles.y + _angleOffset, _speedRotationAround * Time.deltaTime), 0);

        }
    }

    void CalulateAngle()
    {

        if (_forward && !_back)
        {

            if (!_left && _right)
            {
                _angleOffset = 45;
            }


            else if (_left && !_right)
            {
                _angleOffset = -45;
            }
            else
            {
                _angleOffset = 0;
            }


        }
        else if (!_forward && _back)
        {



            if (!_left && _right)
            {
                _angleOffset = 135;
            }
            else if (_left && !_right)
            {
                _angleOffset = -135;
            }
            else
            {
                _angleOffset = 180;
            }
        }

        else
        {

            if (!_left && _right)
            {

                _angleOffset = 90;

            }
            else if (_left && !_right)
            {

                _angleOffset = -90;

            }
        }


    }

}
